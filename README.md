# EmptyKeysGuide
A basic Guide to using EmptyKeys with Monogame

##Requirements:
- Visual Studio 2013 update 2
- [Monogame 3.3](http://www.monogame.net/downloads/)
- [EmptyKeys 1.6b](http://emptykeys.com/Products)

##Basic Guide

- Create a Monogame project
  - Add a Folder called UI to the project

- Extract EmptyKeysUIvxxxx.zip to a folder called EmptyKeys in the same folder that has the sln file
  - Your Folders should now look like this
  - Solution folder
    - EmptyKeys
    - MonoGame Game

- Add a reference to these three dlls in the MonoGame Project
  - EmptyKeys.UserInterface (EmptyKeys/Common/AnyCPU)
  - EmptyKeys.UserInterface.Core (EmptyKeys/Common/AnyCPU)
  - EmptyKeys.UserInterface.MonogGame (EmptyKeys/MonoGame/AnyCPU)

- Create a WPF User Control Library called UserInterface
  - Add a reference to EmptyKeys.UserInterface.Designer (in the folder EmptyKeys/Generator/AnyCPU)

- Right Click The UserInteface Project and click Properties, go down to Build Events and add Edit Post-Build...
  - Add the Following: $(ProjectDir)..\EmptyKeys\Generator\ekUiGen.exe -i=$(ProjectDir) -o=$(ProjectDir)..\<MonoGame Game>\UI -oa=$(ProjectDir)..\<MonoGame Game>\Content -rm=MonoGame
  - Replace "<MonoGame Game>" with the name of your game's project 

- Add a WPF UserControl to the UserInterface Project

- Add EmptyKeys.UserInterface.Designer as an xmlns on the control like so
  - xmlns:ek="clr-namespace:EmptyKeys.UserInterface.Designer;assembly=EmptyKeys.UserInterface.Designer"
  - (Do not worry about the errors saying UIRoot doesn't exist in the namespace)

- Change UserControl to ek:UIRoot and rename the File BasicUI
  - (Do not worry about the error saying type 'ek:UIRoot' was not found)
  - You can delete UserControl.cs from the project, it won't be used 

- Add some basic xaml to the control like this
  ```
  <TextBlock Text="Hello World" Grid.Row="0" />
  ```
  
- Build the UserInterface Project, if it succeded there will be somthing similar in your Output window  
```
========== Build: 1 succeeded or up-to-date, 0 failed, 2 skipped, Completed at 3/31/2015 6:41:56 PM ==========
```

- In the MonoGame project, right click the UI folder and add the generated class(es)

- Double click on Content.mgcb to open the Monogame Content Builder
  - Add the spritefont "Segoe_UI_9_Regular" and any other media you would like by right clicking "content" and pressing "add item" from the content folder in your MonoGame project
  - Press f6 to build the assets, the space on the right of the program will show if it was successful after a few seconds

- Under Game1.cs in the MonoGame project add the following lines
  
``` C#
public class Game1 : Game
    {
        GraphicsDeviceManager graphics;
        
        private BasicUI basicUI;
        private int nativeScreenWidth;
        private int nativeScreenHeight;
        public Client()
            : base()
        {
            graphics = new GraphicsDeviceManager(this);
            Content.RootDirectory = "Content";
            graphics.PreparingDeviceSettings += graphics_PreparingDeviceSettings;
            graphics.DeviceCreated += graphics_DeviceCreated;
        }
        
        void graphics_PreparingDeviceSettings(object sender, PreparingDeviceSettingsEventArgs e)
        {
            nativeScreenWidth = graphics.PreferredBackBufferWidth;
            nativeScreenHeight = graphics.PreferredBackBufferHeight;
            
            // Or any other resolution
            graphics.PreferredBackBufferWidth = 1280;
            graphics.PreferredBackBufferHeight = 720;
            graphics.PreferMultiSampling = true;
            graphics.PreferredDepthStencilFormat = DepthFormat.Depth24Stencil8;
        }
        
        void graphics_DeviceCreated(object sender, System.EventArgs e)
        {
            Engine engine = new MonoGameEngine(GraphicsDevice, nativeScreenWidth, nativeScreenHeight);
        }
        // ... means we skipped some code for brevity, do not remove or change the code that would otherwise be there
        ...
        protected override void LoadContent()
        {
            SpriteFont font = Content.Load<SpriteFont>("Segoe_UI_9_Regular");
            FontManager.DefaultFont = Engine.Instance.Renderer.CreateFont(font);
            
            Viewport viewport = GraphicsDevice.Viewport;
            root = new Root(viewport.Width, viewport.Height);
            
            FontManager.Instance.LoadFonts(Content);
            // Load the images and sounds if necessary
            //ImageManager.Instance.LoadImages(Content);
            //SoundManager.Instance.LoadSounds(Content);
            
            // Replace the escape to close command from the update method
            RelayCommand command = new RelayCommand(new Action<object>(ExitEvent));
            KeyBinding keyBinding = new KeyBinding(command, KeyCode.Escape, ModifierKeys.None);
            root.InputBindings.Add(keyBinding);
        }
        
        private void ExitEvent(object obj)
        {
            Exit();
        }
        ...
        protected override void Update(GameTime gameTime)
        {
            // remove the escape to exit line and add the following
            root.UpdateInput(gameTime.ElapsedGameTime.Milliseconds);
            root.UpdateLayout(gameTime.ElapsedGameTime.Milliseconds);
            base.Update(gameTime);
        }
        
        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);
            root.Draw(gameTime.ElapsedGameTime.TotalMilliseconds);
            base.Draw(gameTime);
        }
  ```

For any help with getting your project setup see (these examples)[https://github.com/EmptyKeys/UI_Examples/tree/master/BasicUI_MonoGame] for more or come to (the forums)[http://emptykeys.com/Community/]

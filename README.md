# EmptyKeysGuide
A basic Guide to using EmptyKeys using Monogame

Requires:
- Visual Studio 2013 update 2
- Monogame 3.3
- EmptyKeys 1.6b

1. Create a Monogame project
2. Extract EmptyKeysUIvxxxx.zip to a folder called EmptyKeys in the same folder that has the sln file
  - Your Folders should now look like this
  - Game folder
    - EmptyKeys
    - MonoGame Game
3. Add a reference to these three dlls in the MonoGame Project
  - EmptyKeys.UserInterface (Under EmptyKeys/Common/AnyCPU)
  - EmptyKeys.UserInterface.Core (Under EmptyKeys/Common/AnyCPU)
  - EmptyKeys.UserInterface.MonogGame (Under EmptyKeys/MonoGame/AnyCPU)
4. Create a WPF User Control Library called UserInterface
  - Add a reference to EmptyKeys.UserInterface.Designer (in the folder EmptyKeys/Generator/AnyCPU)
5. Right Click The UserInteface Project and click Properties, go down to Build Events and add Edit Post-Build...
  - Add the Following: $(ProjectDir)..\EmptyKeys\Generator\ekUiGen.exe -i=$(ProjectDir) -o=$(ProjectDir)..\MonoGame Game\UI -oa=$(ProjectDir)..\MonoGame Game\Content -rm=MonoGamm
6. Add a WPF UserControl to the UserInterface Project
7. add EmptyKeys.UserInterface.Designer as an xmlns on the control like so
  - xmlns:ek="clr-namespace:EmptyKeys.UserInterface.Designer;assembly=EmptyKeys.UserInterface.Designer"
  - (Do not worry about the errors saying UIRoot doesn't exist in the namespace)
8. Change UserControl to ek:UIRoot
  - (Do not worry about the error saying type 'ek:UIRoot' was not found)

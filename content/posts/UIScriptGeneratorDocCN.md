+++
date = '2025-05-27T20:08:26+08:00'
draft = false
title = 'UI Script Generator文档'

+++



# 设置文件

## 文件

* 手动创建设置文件的操作不是必须的，因为在每次生成器激活时，生成器都会去**Editor>UIScriptGenerator**目录查找是否有已存在的设置文件，若未找到则自动在该位置生成一个新的设置文件。
* 若需要手动创建设置文件，在菜单栏中选择：**Tools>UI Script Generator>Create Setting**，将会在**Editor>UIScriptGenerator**目录下创建UIScriptGeneratorSetting.asset文件，其中包含了生成器所需的所有设置数据。
* 设置文件的保存目录是固定的，若你将设置文件转移到其他目录，则生成器会在**Editor>UIScriptGenerator**目录下再创建一个新的。

## 选项

* **Generation Data Save Path**
  * 生成UI预制体脚本的相关数据存储在一个与预制体同名的ScriptObject对象中，这些ScriptObject对象都存储在这个目录。
* **Ui Script Save Path**
* 生成的UI脚本的存储目录。
* **Ui Script Assembly**
* 生成的UI脚本所在的程序集。这将用于查询已生成的脚本对象，并自动将其添加到对应的预制体上，若此设置为空则不会进行自动添加。
* **Ui Script Name Space**
* 生成的UI脚本所在的命名空间。这将用于查询已生成的脚本对象，并自动将其添加到对应的预制体上。此设置依据你的项目代码结构而定，默认命名空间为“UI”，若你自定义的生成器生成的UI代码不包含命名空间，则此设置可以为空。
* **Ui Panel Base Class Name**

  * 面板类型的UI的脚本基类名称。若此设置不为空，则生成UI脚本时将自动继承自该名称对应的类。默认设置中的UIPanelBase是作为示例的基类，没有其他作用，脚本位于UIScriptGenerator>Runtime>UIPanelBase.cs，可根据情况删除（设置中的基类名称也要同步删除）。
* **Ui Widget Base Class Name**

  * 挂件类型UI的脚本基类名称。若此设置不为空，则生成UI脚本时将自动继承自该名称对应的类。默认设置中的UIWidgetBase是作为示例的基类，没有其他作用，脚本位于UIScriptGenerator>Runtime>UIWidgetBase.cs目录，可根据情况删除（设置中的基类名称也要同步删除）。
* **Add Suffix**

  * 是否在变量名之后添加对应组件的名称缩写。此设置的目的是避免在生成同一节点下的多个组件变量时出现变量名冲突。当然你也可以手动修改每个变量名来解决冲突。
* **SuffixList**

  * 组件名称缩写列表。在这里你可以定义每个组件的名称缩写，若未获取到某个组件的名称缩写，则使用组件的完整名称作为后缀。
* **Components Has Callback**
  * 在这里可以定义拥有回调委托的组件，例如Button组件拥有onClick回调，Slider组件拥有onValueChanged回调等等。
  * 只有添加到这个列表中的组件才会显示“Generate Callback Func”选项

# 教程

* **双击一个UI预制体进入预制体编辑窗口**

  * 生成器的大部分功能都是基于预制体的，因此只有在预制体编辑模式下才能激活生成器。并且预制体的根节点必须添加有RectTransform组件，否则也不能激活生成器。

* **在菜单栏中选择Tools>UI Script Generator>Enable**

  * 这将激活生成器，你会在Hierarchy窗口看到预制体根节点右边出现“Exit”、“Save”两个按钮，这代表着生成器激活成功。
  * 点击Exit按钮将退出生成器
  * 点击Save按钮将弹出脚本生成窗口

* **选中任意一个子节点**

  * 选中子节点，在Inspector窗口中可以看到RectTransform组件下方出现了多个复选项，格式为“口 Generate xxx”，它们表示当前节点可以获取到的组件对象。

* **勾选任意一个选项**

  * 在其下方会出现“Variable Name”文本框，其中的文本代表生成的脚本中用于存储该组件对象的变量的名称。你可以随意修改该名称，但不能为空。
  * 若勾选的组件已注册到了设置文件的**Components Has Callback**中，则还会出现“Generate Callback Func”选项。若勾选此选项，在生成脚本时会生成该组件的回调函数并自动添加到回调委托。例如Button组件，名称为Select，则将生成**OnSelectBtnClick**回调函数，并添加到**onClick**委托。
  * 默认设置中的组件及其回调委托

  | 组件                   | 回调委托                                                  |
  | ---------------------- | --------------------------------------------------------- |
  | Button                 | onClick                                                   |
  | Slider                 | onValueChanged                                            |
  | Toggle                 | onValueChanged                                            |
  | Dropdown、TMP_Dropdown | onValueChanged                                            |
  | InputField             | onValueChanged、onEndEdit、onSubmit                       |
  | TMP_InputField         | onValueChanged、onEndEdit、onSubmit、onSelect、onDeselect |
  | ScrollRect             | onValueChanged                                            |

* **保存并生成**

  * 勾选完所有你需要的节点的组件选项后，点击Save按钮，弹出脚本生成窗口
  * 窗口中有三部分内容：
    * **脚本存储目录**：生成的脚本将存储到该目录，默认格式：设置数据中的“UIScriptSavePath”目录/预制体名称，你可以随意修改此目录，它将会保存到当前预制体对应的ScriptObject对象中，之后再生成脚本时将使用已保存的目录。
    * **使用的生成器**：选择生成脚本所使用的生成器，不同的生成器可能会有不同的生成逻辑，生成出来的脚本也将不同。默认提供了两种生成器：UIPanelScriptGenerator、UIWidgetScriptGenerator，分别生成面板UI、挂件UI脚本。你也可以自定义生成器，它将自动出现在选择列表中。与存储目录一样，选择的生成器将会保存到预制体对应的ScriptObject对象中，之后生成时不需要再次选择。
    * **生成按钮**：保存预制体的生成数据、生成脚本文件、自动添加脚本到预制体。
  * 将生成两个脚本文件（下面的 xxx 默认情况是UI预制体名称）
    * **xxxBase.cs**：ui脚本基类，其中包含存储组件对象的变量，获取组件对象的代码，回调函数。每次生成该文件都会被覆盖，所以不要在其中做任何修改。
    * **xxx.cs**：这个文件中只定义了类型，没有其他代码，你将在这个脚本中编写业务逻辑。此文件只会在第一次生成脚本时创建，之后将不会再创建，除非你手动删除了该文件，所以你不需要担心业务代码被覆盖。
  * 默认生成器
    * UIPanelScriptGenerator
      * 用于生成面板类型的UI的脚本，例如背包、排行榜、商店面板等等，这些面板通常由UI管理器创建并控制它们的生命周期。
    * UIWidgetScriptGenerator
      * 用于生成挂件类型的UI的脚本，例如背包面板中每一个展示物品的节点、排行榜面板中每一个名次信息节点等等，这些挂件对象通常是在编写业务代码时手动实例化创建，并在业务逻辑中控制它们的生命周期。
  
* **开始编写业务代码**

  * 完成上面的操作后你便可以开始编写业务代码，你可以直接获取并使用组件变量。需要注意的是默认生成器生成的代码中的组件变量是在Awake函数中赋值的，为了确保获取到的变量已被赋值，你需要在Awake函数执行完成后再获取，例如在Start函数中获取。如果你需要在Awake周期进行某些操作，可以重写OnAwakeEnd函数，它会在所有变量赋值完成后被调用，你所有需要在Awake周期执行的代码都应该写在此函数中，以确保所有变量都被赋值。
  * 若你勾选了某个组件的“Generate Callback Func”选项，在基类脚本中将定义该组件的委托回调函数，它是一个虚函数，所以你可以在编写业务代码是重写它们，并在其中编写代码。这些回调函数在基类脚本获取组件变量时已自动添加到组件对象的委托中，你不需要再手动添加。


# 自定义

## Inspector窗口

* 默认情况下所有组件都使用的同一个绘制类（DefaultDrawer）
* 你可以为每一种UI组件创建单独的绘制类，自定义Inspector中的绘制样式
* 所有绘制类都必须继承自DrawerBase抽象类，它包含两部分内容
  * ComponentType：代表当前绘制类处理的UI组件类型，你需要为其赋予一个Type类型的值，否则生成器不会搜集到该绘制类。
  * DrawGUI接口：你需要在此接口中实现自定义的绘制逻辑，它有两个参数
    * ComponentGenerationData
    * Type
* 完成以上步骤后就无需额外操作，生成器激活时会自动搜集所有绘制类并实例化它们
* 注意：一个UI组件类型只应该有一个自定义的绘制类，若存在多个也只会有一个被使用

## 生成器

* 你可以创建任意数量的自定义生成器，用于生成不同需求的脚本文件
* 所有生成器类都必须实现IScriptGenerator接口，它只有一个函数：GenerateScript(UIPrefabGenerationData data, out string baseClassContent, out string uiScriptContent);
* 你需要在此接口中实现脚本生成逻辑，并返回两个字符串，分别是基类脚本内容、子类脚本内容，生成器将使用这两个字符串生成对应的脚本文件。
* 与自定义Inspector窗口一样，创建好自定义生成器类后无需额外操作，生成器激活时会自动收集所有生成器类型并实例化它们，你可以在生成窗口的Generator选项中看见它们。

# 常见问题

## 选中节点后Inspector中没有显示组件选项

生成器会检查选中的节点是否属于预制体的子节点，若不是则不会进行Inspector绘制。这里用于检查的预制体是位于Project窗口中的资源文件，而不是Hierarchy窗口中正在编辑的预制体。若你选中的节点是新增的，并且未保存，则Inspector窗口不会进行绘制。勾选Auto Save，或者Ctrl+S手动保存，然后重新选中节点来解决此问题。

## 修改节点的位置、名称

修改节点位置或名称后，生成器激活时会自动刷新该节点的路径数据，但不会刷新变量名称，以防止脚本中已使用该变量名而导致报错。若需要刷新变量名，可以取消再勾选对应的组件选项，此时变量名会使用修改后的节点的名称。刷新路径、变量名后都需要再次生成脚本。

## 同一父节点下存在同名子节点

该生成器生成的代码都使用全路径在运行时查找节点并获取节点的组件对象，因此无法识别同一父节点下的同名子节点，这在勾选组件选项时也能发现（同名节点的Inspector绘制信息不会变化，因为它们使用了同一个生成数据对象）。所以需要保证拥有同一父节点的节点中不存在同名节点，否则运行时获取到的节点可能不是期望的节点而出现错误。

## 无法激活生成器

解决办法：

* 在菜单栏中选择**Tools>UI Script Generator>Disable**，手动禁用生成器，然后再激活
* 重启Unity编辑器
* 删除预制体对应的生成数据文件（与预制体同名，存储在设置文件**Generation Data Save Path**选项定义的目录中），然后重新激活

## 第一次生成时无法自动添加脚本到预制体

* 检查**Ui Script Assembly**、**Ui Script Name Space**设置是否正确
* 可能设置的程序集还未被加载，尝试再次生成脚本

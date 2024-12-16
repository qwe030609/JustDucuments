
# UI Recorder Enhancement

## Results
### Original XPath Code from UI Recorder

#### Longer XPath
```csharp
/Pane[@ClassName="#32769"][@Name="桌面 1"]/Window[@ClassName="Window"][@Name="Chroma ATS IDE - [TI Editor]"]/Custom[@AutomationId="Container"]/Pane[@ClassName="ScrollViewer"]/Custom[@ClassName="MainControl"]/Group[@Name="dockManager"][@AutomationId="dockManager"]/Group[@Name="dockItem4"][starts-with(@AutomationId,"dockItem")]/Group[@Name="dockItem1"][starts-with(@AutomationId,"dockItem")]/Group[@Name="dockItem2"][starts-with(@AutomationId,"dockItem")]/Group[@Name="dockItem3"][starts-with(@AutomationId,"dockItem")]/Group[@Name="TIContextPanel"][@AutomationId="TIContextPanel"]/Custom[@ClassName="PGGridAeraView"]/DataGrid[@AutomationId="PGGrid"]/Pane[@Name="DataPanel"][@AutomationId="dataPresenter"]/DataItem[starts-with(@Name,"Chroma.TestItemEditor.ComposedElement.Default.PGGridRowViewModel")]/Custom[starts-with(@Name,"ReadAC_Current, Item: Chroma.TestItemEditor.ComposedElement.Defa")][@AutomationId="CommandName"]
```

#### Shorter XPath
```csharp
/DataItem[starts-with(@Name,"Chroma.TestItemEditor.ComposedElement.Default.PGGridRowViewModel")]/Custom[starts-with(@Name,", Item: Chroma.TestItemEditor.ComposedElement.Default.PGGridRowV")][@AutomationId="CommandName"]
```

### Modified Code with Specific String Formats

#### Record Single Left Click
```csharp
PP5IDEWindow.PerformClick("/ById[searchText]", ClickType.LeftClick);
```

#### Record Multiple Left Clicks
```csharp
PP5IDEWindow.PerformClick("/ById[searchText]", ClickType.LeftClick);
PP5IDEWindow.PerformClick(, ClickType.LeftClick); // failed, no identifiers recorded
PP5IDEWindow.PerformClick("/ById[btnYes]", ClickType.LeftClick);
PP5IDEWindow.PerformClick("/ById[Close]", ClickType.LeftClick);
```

#### Modified Code with Specific String Formats
```csharp
// 0924, modified code:
PP5IDEWindow.PerformClick("/ByClass[PGGridAeraView]/ById[PGGrid,dataPresenter]", ClickType.LeftClick);
PP5IDEWindow.PerformClick("/ById[PGGrid,dataPresenter,CommandName]", ClickType.LeftClick);
PP5IDEWindow.PerformClick("/ById[editParamAreaView,ParameterGrid,dataPresenter]", ClickType.LeftClick);
PP5IDEWindow.PerformClick("/ById[editParamAreaView]/ByClass[ScrollViewer,TextBox]", ClickType.LeftClick);
```

## Functions
1. Can record the actions and format as the __*specified string formats*__ section.
2. Skip XPath parts that start with: `starts-with`, `contains`.
3. Currently supported actions: `LeftClick`, `RightClick`, `LeftDoubleClick`.

## TODOs
1. Comment support from UI Recorder:
   - `// from UI recorder: locate By identifiers`
   - `// from UI recorder: locate By control extension methods`
   - `// from UI recorder: locate By normal methods`

2. Add support for formatting:
   - **By control extension methods:** `MenuSelect("Functions", "TI Editor")`, `ToolBarSelect()`, ...
   - **By normal methods:** `(/Window[...]/Text[...])`
   - **By DataGrid related methods:** `(/Window[...]/Text[...])`
   - **By complex actions of an extension method:** 
     - Test Command related: `AddCommand`, `GetCommand`, ...
     - Parameters related: `CreateNewVariable1(params)`, `CreateNewVariable2(params)`

3. Add support for action **InputType formatting style**:
   1. Normal send keys: `"sendContent"`
   2.  Special actions:
       - Ctrl + A: Select All
       - Ctrl + C: Copy
       - Ctrl + D: Paste
       - ...

## Specific String Formats

### Record Click Action
```csharp
PP5IDEWindow.PerformClick({LocatorPath}, {ClickTypes})
```
- **LocatorPath:** see "PerformGetElement" section.
- **ClickTypes:** `ClickType.LeftClick`, `InputType.Send`, ...
  
**Example:**
```csharp
PP5IDEWindow.PerformClick("/ById[TCListPanel]/ById[PreviosBtn]", ClickType.LeftClick);
PP5IDEWindow.PerformClick("/ByCell@PGGrid[1,@Skip]", ClickType.TickCheckBox);
```

### Record Input Action
```csharp
PP5IDEWindow.PerformInput({LocatorPath}, {InputTypes}, {textToInput})
```
- **LocatorPath:** see "PerformGetElement" section.
- **InputTypes:** `InputType.SelectAllContent`, `InputType.SendContent`, ...
- **textToInput:** null or "AnyText".

**Example:**
```csharp
PP5IDEWindow.PerformInput("/ByCell@PGGrid[2,@TestCommand]", InputType.SelectAllContent, null);
PP5IDEWindow.PerformInput("/ById[TCListPanel]/ById[SearchBox]", InputType.SendContent, "ReadAC_Current");
```

### Record Inspect Action
```csharp
PP5IDEWindow.PerformGetElement({LocatorPath})
```
- **LocatorPath Examples:**
  - `"/Window[Search Result]/ById[txtBlockMsg]"`
  - `"/ById[TCListPanel]/ById[searchText]"`
  - `"/ByCell@PGGrid[1,@Description]"`

- **LocatorPath Types:**
  1. `"/Byxxx[Locator1,Locator2,Locator3,...]"` (Single By identifier with multiple element identifiers)
  2. `"/Byxxx[Locator1]/Byyy[Locator2]"` (Multiple By identifiers with Single/multiple element identifiers)
  3. `"/Window[Locator1]/Button[Locator2]"` (ControlType as identifier)
  4. `"/Window[Locator1]/Byxxx[Locator2]/Byyy[Locator1,Locator2,Locator3,...]"` (Mixed of above 1~3)

**Example:**
```csharp
IElement ele = PP5IDEWindow.PerformInput("/ById[TCListPanel]/ById[searchText]", "/ByCell@PGGrid[1,@Description]");
```
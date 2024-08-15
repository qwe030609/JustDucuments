在 MS Test 中，你可以使用 `DeploymentItem` 特性以及 `DeploymentItem` 屬性搭配 `Deployment` 屬性來將資料文件（如 CSV 或 Excel）嵌入到測試專案中。這樣，在執行測試時，相關的資料檔案就會被部署到執行目錄中，並且可以在 `DataSource` 屬性中直接使用。

以下是一個簡單的例子：

1. 創建一個資料檔案（例如 `TestData.csv`）：

```csv
input1,input2,expected
1,2,3
3,5,8
```

2. 在測試專案中，為這個資料檔案加上 `DeploymentItem` 特性：

```csharp
[TestClass]
public class MyTestClass
{
    [TestMethod]
    [DeploymentItem("TestData.csv")] // 資料檔案的路徑相對於測試專案的根目錄
    [DataSource("Microsoft.VisualStudio.TestTools.DataSource.CSV", "|DataDirectory|\\TestData.csv", "TestData#csv", DataAccessMethod.Sequential)]
    public void MyTestMethod(int input1, int input2, int expected)
    {
        // 測試邏輯
        int result = YourClass.Add(input1, input2);

        // 驗證結果
        Assert.AreEqual(expected, result);
    }
}
```

3. 在測試方法中，使用 `DeploymentItem` 特性標記資料檔案的路徑。`|DataDirectory|` 表示部署目錄。

這樣，當你執行測試時，`TestData.csv` 就會被部署到執行目錄中，並且可以在 `DataSource` 屬性中使用。

注意：這個方法會將資料檔案嵌入到測試專案的輸出目錄中，因此在執行測試時需要確保相關的資料檔案已經部署。在使用此方法時，測試專案的資料夾結構可能會影響路徑。確保檔案路徑的正確性是很重要的。
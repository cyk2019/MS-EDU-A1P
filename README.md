# 微软A1P批量创建用户脚本

**原作者lzdszdl的脚本**

1. 管理员身份运行powershell

2. 分别执行以下命令

    ```
    Install-Module -Name AzureAD
    Install-Module -Name MSOnline
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
    ```

3. 登录账号

    ```
    Connect-AzureAD
    Connect-MsolService
    ```

4. 获取SKU并复制SKU

    ```
    Get-MsolAccountSku
    ```

5. 修改以下脚本

    ```powershell
    Function GetRandom($NUM,$RT){
    If ([String]::IsNullOrEmpty($NUM)) { Return } Else { If ([String]::IsNullOrEmpty($($($NUM).ToString()).Trim())){ Return }}
    If ($NUM -match ",") {$Len1 = (($NUM -split(",", 2))[0]).Trim(); $Len2 = (($NUM -split(",",2))[1]).Trim()} Else{$Len = $NUM; $Len1 ="";$Len2 =""}
    If (-Not ([String]::IsNullOrEmpty($Len1) -or [String]::IsNulorEmpty($Len2))) { $Len = ((([convert]::ToInt32($Len1,10)).. ([convert]::ToInt32($Len2,10)))| Get-Random) }
    If ([String]::IsNullOrEmpty($Len)) { Return }
    $RList = (49..57 + 65..90 + 97..122)
    Return -join( $RList | Get-Random -count $Len| %{[char]$_})
    }
    
    #注意：5000为批量注册的用户总数
    for($i=1;$i -le 5000;$i++)
    {   $passwd=GetRandom(16)
        $passwd=$passwd+"!"
        #$head=GetRandom(8)
        #自行定义用户名前缀 域名自行查看
        $name="用户名前缀"+$i+"@你的域名"
        #修改 xxxxx:STANDARDWOFFPACK_STUDENT 为自己的SKU
        New-MsolUser -DisplayName O365 -UserPrincipalName $name -UsageLocation US -Password $passwd -LicenseAssignment xxxxx:STANDARDWOFFPACK_STUDENT
        #D:\A1P_STU.txt 为用户账密存放路径
        "$name--$passwd" | Out-File -Append D:\A1P_STU.txt
    }
    ```

6. 保存为xx.ps1格式，在powershell执行即可

    

    
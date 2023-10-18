# Mã hoá và bảo mật cho mật khẩu


## Mã hoá (sử dụng entrophy)
```cs
var passwordInByte = Encoding.UTF8.GetBytes(password);
var entropy = new byte[20];
using(var rng =  RandomNumberGenerator.Create()) {
    rng.GetBytes(entropy);
}

var cyphertext = ProtectedData.Protect(passwordInByte, entropy, DataProtectionScope.CurrentUser);

config.AppSettings.Settings["password"].Value = Convert.ToBase64String(cyphertext);
config.AppSettings.Settings["entropy"].Value = Convert.ToBase64String(entropy);


Configuration config = ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);

config.Save(ConfigurationSaveMode.Minimal);
ConfigurationManager.RefreshSection("appSettings");
```

## Giải mã
```cs
var cyphertext = Convert.FromBase64String(ConfigurationManager.AppSettings["password"]);
var entropy = Convert.FromBase64String(ConfigurationManager.AppSettings["entropy"]);
var cypherInByte = ProtectedData.Unprotect(cyphertext, entropy, DataProtectionScope.CurrentUser);
var password = Encoding.UTF8.GetString(cypherInByte);
// gán lại vào màn hình login
txtPassword.Text = password;
```

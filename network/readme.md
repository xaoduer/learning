keytool -genkey -alias d_server -keyalg RSA -keystore d_server.jks -validity 720 -storepass 123456
keytool -export -alias d_server -file d_server.cer -keystore d_server.jks -storepass 123456

keytool -genkey -alias d_client -keyalg RSA -keystore d_client.jks -validity 720 -storepass 654321
keytool -export -alias d_client -file d_client.cer -keystore d_client.jks -storepass 654321

## 服务端trustStorePath：d_client_for_server.jks
keytool -import -alias d_client -file d_client.cer -keystore d_client_for_server.jks


D:\project-spring\zte\https>keytool -printcert -rfc -file d_server.cer
-----BEGIN CERTIFICATE-----\r\n
MIIDQzCCAiugAwIBAgIEBQTLUDANBgkqhkiG9w0BAQsFADBSMQswCQYDVQQGEwJjbjELMAkGA1UECBMCY2QxCzAJBgNVBAcTAmNkMQwwCgYDVQQKEwN6dGUxDDAKBgNVBAsTA3p0ZTENMAsGA1UEAxMEZHV5bDAeFw0xOTA4MTIwNjQ4NTlaFw0yMTA4MDEwNjQ4NTlaMFIxCzAJBgNVBAYTAmNuMQswCQYDVQQIEwJjZDELMAkGA1UEBxMCY2QxDDAKBgNVBAoTA3p0ZTEMMAoGA1UECxMDenRlMQ0wCwYDVQQDEwRkdXlsMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAk7YjCiZKUZM2Xnz2KPq5pnkoJUyCxbQ3cGHRJI2Zsw2FqW4Ed7nenx0NNfP2TeDYTFT3Xtgw93E98p5EGUsgkFjromeQsMKw5vuxUFu5wvQ1PrPWqc6m5b4fAnhL+Es9vAaLl2KCmeRjMztec/Syg3LWys3yMbOz51UkyDb1aSstauTFkdNYqCupxSKHQRELeY/qZuiOdTGA6+UAniQymQpy04/skGLKs246RNiKJI4XVCbHaQacpLHK2lXpyA+VpXXJmCShLESgPfI/x20P8BWf0uNH941Grd1yOxIgAXptTJ1KkJuwMKBBxuJoj1cADDeBxN5/CuxkaeCiggsvuQIDAQABoyEwHzAdBgNVHQ4EFgQUGkWEqxcEV/mBDaFQKb2/5wUYWTMwDQYJKoZIhvcNAQELBQADggEBAESD0R7fZHYpGlYv9svojiv0HOCnxXqVYFvustvWNx4gnlenf8iIV58wwGlOfIkrjA84hoknnAiyPim9zZ8GWAPQVh36CRq60SntzlD4P9H1WXdrnsl2knhNHwvLpQU0w8BUIOeBp0Wqbcc5eRMkqIo1sPFCGqeR3K6tkT2WFVhGNVX32Z0UbwJXUP3EQhv+yiK5B6w2K2ulFexDLsljWsory81X385iAdGnlPd7S0z+OgRfDqkTy7WEdZRYnIQLdDHI+ZVqiv5vkxLnxWbpj5eqT+812ubiLzGY6kV6/yoYNDPFW2BwAIY7VQIkWUU3QPHII12M4Gi8KmcV6wp0KM0=
-----END CERTIFICATE-----


client:
openssl pkcs12 -export -in client.crt -inkey client.key -out client.pkcs12
123456

keytool -importkeystore -srckeystore client.pkcs12 -destkeystore client.jks -srcstoretype pkcs12
123456
123456
123456

keytool -importcert -keystore client.jks -file ../root/ca.crt
123456

truststore:
keytool -importcert -alias ca -file ../root/ca.crt -keystore clienttrust.jks
123456
keytool -importcert -alias clientcert -file client.crt -keystore clienttrust.jks
123456


server:
openssl pkcs12 -export -in cert.crt -inkey cert.key -out cert.pkcs12
654321

keytool -importkeystore -srckeystore cert.pkcs12 -destkeystore cert.jks -srcstoretype pkcs12
654321
654321
654321

keytool -importcert -keystore cert.jks -file ../root/ca.crt
654321

trust:
keytool -importcert -alias ca -file ../root/ca.crt -keystore certtrust.jks
654321
keytool -importcert -alias certcert -file cert.crt -keystore certtrust.jks
654321

keytool -v -list -keystore cert.jks

.crt:
-----BEGIN CERTIFICATE-----\r\n
MIID1jCCAr6gAwIBAgIJAJlCl55Vt6IRMA0GCSqGSIb3DQEBCwUAMIGKMQswCQYDVQQGEwJDTjERMA8GA1UECAwIc2ljaHVhbmcxEDAOBgNVBAcMB2NoZW5nZHUxDDAKBgNVBAoMA3p0ZTEOMAwGA1UECwwFemVuYXAxODA2BgNVBAMML1pURSBPcGVuUGFsZXR0ZSBSb290IENlcnRpZmljYXRlIEF1dGhvcml0eSAyMDE3MB4XDTE5MDgxNDE1MDEzNVoXDTI5MDgxMTE1MDEzNVowgYgxCzAJBgNVBAYTAkNOMREwDwYDVQQIDAhzaWNodWFuZzEQMA4GA1UEBwwHY2hlbmdkdTEMMAoGA1UECgwDenRlMQ4wDAYDVQQLDAV6ZW5hcDEhMB8GA1UEAwwYKi5vcGVucGFsZXR0ZS56dGUuY29tLmNuMRMwEQYJKoZIhvcNAQkBFgRudWxsMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA3dY5KnQSgSgtEW1JLWc6gxInjxVSDmDhdxiT5xYL7mktihBzyHrbmHJ0m16zDVT1/ltFYdIeqEgN5kPgfVPfDXEYlgNLcuxjj/tBIFXWXrsXkGaFN5xYrjZO6nxnMuEYP7snLKK5EPRQou5mjKWxT6OiTk7M4AsNE8MJ1cCEenVXEf0dIzpaVYdDGk9qM/SNFF6+Er4DDuLMCB2rD56N186wi7BOqPYNbNwhSO5ElChpZ7FCfrbadfk9bSnPqOf33IiSrrqLFxvLIOWiETYzMCA3btmd3uxDTb9tRd2vp44l9Iird+YgvAPBAeYhEZLAupzzf1oaqcYZIKoekg+LBQIDAQABoz8wPTAJBgNVHRMEAjAAMAsGA1UdDwQEAwIF4DAjBgNVHREEHDAaghgqLm9wZW5wYWxldHRlLnp0ZS5jb20uY24wDQYJKoZIhvcNAQELBQADggEBAFfXUcH3lXXpgM5XTSFpqFU/BB7Qaf+gBHO/P3j6KsQy1qZfps3Fua+7WQqSaVCULcCEajYBzfumOlgPHn3++a3rEHssemD1yOUkBz+2jI3KmknNZ3U72hvrYNJSFK9i3RpRGIov8Sef1oQ3S8oo5l09k52XcBR118NbBSGB6GKeEMJ3hlAbTUQba0aHN2QAwS8kOAwz+YhAEL4M3YwYSuOvB1fYphdykCRgqdSPDDjJo9DF6jzL1DEEcN/LC5WiDjxoWdLG8DhIr1wD7Dk7NFQ0/yduNg81/HQzUFi+wMXUiHyz4rbz3fA7SsG49mppKL31K+trui5eLANO+E0MuGM=
-----END CERTIFICATE-----
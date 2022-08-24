## How to encrypt your text messages in flutter using Encrypt package


1. Install Encrypt package from [https://pub.dev/packages/encrypt](Link)
2. Import your package import 'package:encrypt/encrypt.dart' as enc;
3. Use the below code for encryption and decryption
4. Point to remember key length should be either 32 lengths,64 length and 128 length



```
void encryption(String text) {
    final key = enc.Key.fromUtf8('my 32 length key................');

    final iv = enc.IV.fromLength(1);

    final encrypter = enc.Encrypter(AES(key));

    final encrypted = encrypter.encrypt(text, iv: iv);

    setState(() {
      encryptedText = encrypted.base64;
    });
  }

  void decrypt(String text) {
    final key = enc.Key.fromUtf8('my 32 length key................');

    final iv = enc.IV.fromLength(1);
    final encrypter = enc.Encrypter(AES(key));
    final decrypted = encrypter.decrypt64(text, iv: iv);
    setState(() {
      decryptedText = decrypted;
    });
  }
``` 

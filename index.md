## Welcome to Yapam

As the name suggests, yapam is simply yet another password manager, but a little different.

### "Architecture"

Yapam consists of two (three) main parts: server and client. The third part ([keycloak]) is used just for authentication
and can be replaced with any other authorization server in the future.

The [server] basically is a big key-value-store for users and secrets, and can also hold the users public key that is 
used for encrypting the secrets.

The client will be able to synchronize the secrets with the server and decrypt them with its private key.

### Security

#### Oauth

I decided to not implement the authorization by myself, but use a market ready open source solution: keycloak.

#### Encryption and Decryption

I chose [openPGP] for encryption, as it allows the usage of multiple public keys to encrypt the symmetric key. Therefor 
you can share your secrets with others simply by adding their public key to the encryption mechanism. 

### The secrets

The model of a secret differs between client and server. Both of them share the title and some common properties like 
version or creationDate. The most important property, the data of the secret, is (after decryption) fully exposed on the
client, but encrypted on the server.

#### Server model

```json
{
  "creationDate": "2019-08-26T06:35:44.232Z",
  "data": "somesecretencrypteddata",
  "secretId": "39d09caa-f1e7-4f14-bebe-d471d5213527",
  "title": "title",
  "type": "LOGIN",
  "version": 0
}
```

#### Client model

```json
{
  "creationDate": "2019-08-26T06:35:44.232Z",
  "data": {
    "username": "username",
    "password": "password",
    "url": "http://localhost:8080",
    "notes": "geheim"
  },
  "secretId": "39d09caa-f1e7-4f14-bebe-d471d5213527",
  "title": "title",
  "type": "LOGIN",
  "version": 0
}
```

#### Types

Currently there are fife different secret types with their own specific data fields:

* CREDITCARD: Name on card, cardnumber, expirationDate, cvc, notes
* LOGIN: username, password, url, notes
* ID: Name, Street, Number, zip code, city, country, notes
* WIFI: ssid, password, notes
* NOTE: notes

[keycloak]: https://www.keycloak.org/
[server]: https://github.com/kraddatz/yapam-server
[openPGP]: https://www.openpgp.org/

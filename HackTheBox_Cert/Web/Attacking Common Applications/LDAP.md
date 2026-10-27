`LDAP` (Lightweight Directory Access Protocol) is `a protocol` used to `access and manage directory information`. A `directory` is a `hierarchical data store` that contains information about network resources such as `users`, `groups`, `computers`, `printers`, and other devices. LDAP provides some excellent functionality:

| **Functionality**         | **Description**                                                                                                                                                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Efficient`               | Efficient and fast queries and connections to directory services, thanks to its lean query language and non-normalised data storage.                                                                                 |
| `Global naming model`     | Supports multiple independent directories with a global naming model that ensures unique entries.                                                                                                                    |
| `Extensible and flexible` | This helps to meet future and local requirements by allowing custom attributes and schemas.                                                                                                                          |
| `Compatibility`           | It is compatible with many software products and platforms as it runs over TCP/IP and SSL directly, and it is `platform-independent`, suitable for use in heterogeneous environments with various operating systems. |
| `Authentication`          | It provides `authentication` mechanisms that enable users to `sign on once` and access multiple resources on the server securely.                                                                                    |

| LDAP                                                                                                                                       | Active Directory (AD)                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| A `protocol` that defines how clients and servers communicate with each other to access and manipulate data stored in a directory service. | A `directory server` that uses LDAP as one of its protocols to provide authentication, authorisation, and other services for Windows-based networks.                                                         |
| An `open and cross-platform protocol` that can be used with different types of directory servers and applications.                         | `Proprietary software` that only works with Windows-based systems and requires additional components such as DNS (Domain Name System) and Kerberos for its functionality.                                    |
| It has a `flexible and extensible schema` that allows custom attributes and object classes to be defined by administrators or developers.  | It has a `predefined schema` that follows and extends the X.500 standard with additional object classes and attributes specific to Windows environments. Modifications should be made with caution and care. |
| Supports `multiple authentication mechanisms` such as simple bind, SASL, etc.                                                              | It supports `Kerberos` as its primary authentication mechanism but also supports NTLM (NT LAN Manager) and LDAP over SSL/TLS for backward compatibility.                                                     |

### Commands
```shell
ldapsearch -H ldap://ldap.example.com:389 -D "cn=admin,dc=example,dc=com" -w secret123 -b "ou=people,dc=example,dc=com" "(mail=john.doe@example.com)"
```
- Connect to the server `ldap.example.com` on port `389`.
- Bind (authenticate) as `cn=admin,dc=example,dc=com` with password `secret123`.
- Search under the base DN `ou=people,dc=example,dc=com`.
- Use the filter `(mail=john.doe@example.com)` to find entries that have this email address.

|Input|Description|
|---|---|
|`*`|An asterisk `*` can `match any number of characters`.|
|`( )`|Parentheses `( )` can `group expressions`.|
|`\|`|A vertical bar `\|` can perform `logical OR`.|
|`&`|An ampersand `&` can perform `logical AND`.|
|`(cn=*)`|Input values that try to bypass authentication or authorisation checks by injecting conditions that `always evaluate to true` can be used. For example, `(cn=*)` or `(objectClass=*)` can be used as input values for a username or password fields.|
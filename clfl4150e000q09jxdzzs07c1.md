---
title: "5 Tips for Secure Coding in Python"
datePublished: Thu Mar 23 2023 12:49:02 GMT+0000 (Coordinated Universal Time)
cuid: clfl4150e000q09jxdzzs07c1
slug: secure-coding-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1679575570587/26018fce-d174-4213-baf8-1123c9c11e5b.png
tags: programming-blogs, python, python3

---

Python is a widely used programming language, known for its simplicity, readability, and ease of use. While Python has many benefits, it's important to consider security when writing code. In this blog, we'll discuss some of the key principles of secure coding in Python.

* **Input Validation:** One of the most common ways that attackers can exploit software is through input validation vulnerabilities. Input validation is the process of checking user input to ensure that it conforms to expected values and formats. When accepting user input, it is important to validate that it meets the expected criteria and reject any input that doesn't. This can prevent attackers from sending malicious input to the program, which could cause it to malfunction or give the attacker unintended access to the system.
    

Here's an example of how to validate user input in Python:

```python
import re

def validate_email(email):
    # Check if email matches the expected format
    if not re.match(r"[^@]+@[^@]+\.[^@]+", email):
        raise ValueError("Invalid email address")
```

* **Use Secure Libraries:** Python has a vast library of modules and libraries that can be used to simplify coding tasks. However, not all libraries are created equal, and some may have vulnerabilities that could be exploited by attackers. When choosing libraries to use in your code, it's important to research their security history and use trusted sources. Additionally, keep libraries up to date with the latest security patches and version releases.
    
* **Avoid Hardcoded Credential:** Hardcoding credentials in your code can make them vulnerable to attackers who can easily find them. Instead, use environment variables or configuration files to store sensitive information. This way, if the code is compromised, the credentials won't be easily accessible to attackers.
    

Here's an example of how to store credentials in a configuration file:

```python
import configparser

config = configparser.ConfigParser()
config.read('config.ini')

username = config.get('Credentials', 'username')
password = config.get('Credentials', 'password')
```

* **Encrypt Sensitive Data:** When storing sensitive data, such as passwords or credit card information, it's important to encrypt it to protect it from attackers. Python has several encryption libraries, such as cryptography, that can be used to encrypt data. Additionally, be sure to store encryption keys securely to prevent attackers from accessing them.
    

Here's an example of how to encrypt and decrypt data using the cryptography library:

```python
from cryptography.fernet import Fernet

key = Fernet.generate_key()
cipher = Fernet(key)

data = b"Sensitive data"
encrypted_data = cipher.encrypt(data)
decrypted_data = cipher.decrypt(encrypted_data)
```

* **Use Strong Passwords:** If your code involves user authentication, it's important to use strong passwords to prevent attackers from guessing them. Encourage users to choose strong passwords by enforcing password complexity requirements, such as minimum length and a mix of uppercase and lowercase letters, numbers, and symbols.
    

However, like any other programming language, Python also has its own set of insecure functions that can pose a security threat to your application. Let's discuss five insecure functions and their alternatives that you can use to improve the security of your Python code.

1. **os.system():** The os.system() function is used to execute shell commands from within a Python program. However, it can be a potential security threat as it allows an attacker to execute arbitrary shell commands. An alternative to os.system() is the subprocess module, which provides a more secure way of executing shell commands. You can use the subprocess.call() function to execute shell commands and pass arguments securely.
    
2. **eval():** The eval() function is used to evaluate a string as a Python expression. However, it can be a potential security threat as it allows an attacker to execute arbitrary code. An alternative to eval() is the ast.literal\_eval() function, which evaluates a string as a Python literal expression. It only evaluates a limited set of expressions and is safer than eval().
    
3. **pickle.load():** The pickle.load() function is used to deserialize Python objects from a string. However, it can be a potential security threat as it allows an attacker to execute arbitrary code. An alternative to pickle.load() is the JSON module, which can be used to serialize and deserialize Python objects in a safer way. JSON only supports a limited set of data types and is safer than pickle.
    
4. **urllib.urlopen():** The urllib.urlopen() function is used to open a URL and retrieve its contents. However, it can be a potential security threat as it allows an attacker to execute arbitrary code. An alternative to urllib.urlopen() is the requests module, which provides a more secure way of opening URLs. You can use the requests.get() function to retrieve the contents of a URL securely.
    
5. **md5():** The md5() function is used for hashing data in Python. However, it is insecure as it is vulnerable to collision attacks. Instead of using md5(), you can use the hashlib module to generate secure hashes of data.
    
    Example:
    
    ```python
    import hashlib
    hash_object = hashlib.sha256(b'Hello World')
    hex_dig = hash_object.hexdigest()
    print(hex_dig)
    ```
    

In conclusion, writing secure Python code is an essential aspect of software development. By following best practices and avoiding insecure functions and modules, developers can significantly reduce the risk of security vulnerabilities in their code. The tips outlined in this blog, such as using the subprocess module instead of os.system() and using ast.literal\_eval() instead of eval(), can help developers write secure and safe Python code. Additionally, staying up to date on the latest security trends and practices and regularly updating dependencies can also help ensure the security of your code. By keeping these tips in mind and being vigilant about security, developers can help protect their applications and their users from potential security risks.
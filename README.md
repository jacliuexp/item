Python
```
import requests
response = requests.get('https://www.google.com', verify=False)
print(response.status_code)
print(response.text)
```

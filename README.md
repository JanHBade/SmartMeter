

# Nützliche Links

Mit Login: https://<WLAN Ip vom Pi>/cgi-bin/hanservice.cgi

Mit Frage nach zertifkat. https://<WLAN Ip vom Pi>

# python Beispiel

```
#!/usr/bin/env python3

import requests, base64
from requests.auth import HTTPDigestAuth
from bs4 import BeautifulSoup
import urllib3
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

user= '12345678'
password = 'secret'
url = 'https://192.168.1.200/cgi-bin/hanservice.cgi'
s = requests.Session()
res=s.get(url,auth=HTTPDigestAuth(user, password),verify=False)
cookies = { 'Cookie' : res.cookies.get('session')}

soup = BeautifulSoup(res.content, 'html.parser')
tags = soup.find_all('input')
token = tags[0].get('value')
action = 'meterform'
post_data = "tkn=" + token + "&action=" + action 

res = s.post(url, data=post_data,cookies=cookies,verify=False)

soup = BeautifulSoup(res.content, 'html.parser')
sel = soup.find(id='meterform_select_meter')
meter_val = sel.findChild()
meter_id = meter_val.attrs.get('value')
post_data = "tkn=" + token + "&action=showMeterProfile&mid=" + meter_id

res= s.post(url, data=post_data,cookies=cookies,verify=False)

soup = BeautifulSoup(res.content, 'html.parser')
table_data = soup.find('table', id="metervalue")
result_data = {
                'value': table_data.find(id="table_metervalues_col_wert").string,
                'unit': table_data.find(id="table_metervalues_col_einheit").string,
                'timestamp': table_data.find(id="table_metervalues_col_timestamp").string,
                'isvalid': table_data.find(id="table_metervalues_col_istvalide").string,
                'name': table_data.find(id="table_metervalues_col_name").string,
                'obis': table_data.find(id="table_metervalues_col_obis").string
            }
print(result_data['timestamp']+ " " + result_data['value']+ " " + result_data['unit'])
s.close()
```

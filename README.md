远程控制：手机一键启停 GitHub Actions
既然你希望在国内发送 Web 请求就能自动运行，这里是为你准备的 Python 脚本（你可以运行在电脑或手机 Termux 上）：


import requests

# 配置你的信息
TOKEN = "你的_GITHUB_PERSONAL_ACCESS_TOKEN"
REPO = "你的用户名/你的仓库名"

def control_proxy(action="start"):
    url = f"https://api.github.com/repos/{REPO}/dispatches"
    headers = {
        "Authorization": f"token {TOKEN}",
        "Accept": "application/vnd.github.v3+json"
    }
    # 这里的 start_proxy 对应 YAML 里的 event_type
    payload = {"event_type": "start_proxy" if action == "start" else "stop_proxy"}
    
    response = requests.post(url, headers=headers, json=payload)
    if response.status_code == 204:
        print(f"✅ 指令已发送: {action}")
    else:
        print(f"❌ 失败: {response.status_code}, {response.text}")

# 使用方法: 
# control_proxy("start")  # 启动

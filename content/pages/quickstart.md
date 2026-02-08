# WhatsApp Viral Content Agent for 032576667191
import requests, time, json, os
from datetime import datetime

class WhatsAppAgent:
    def __init__(self):
        self.number = "923257667191"
        
    def start(self):
        print(f"🤖 WhatsApp Agent Started")
        print(f"📱 Target: {self.number}")
        print("⏰ Running every 2 hours\n")
        
        while True:
            self.send_ai_content()
            time.sleep(7200)  # 2 hours
    
    def send_ai_content(self):
        # Get trending AI content
        content = self.get_reddit_ai()
        
        for item in content[:3]:
            message = self.create_message(item)
            self.save_to_file(message)
            print(f"📨 Saved: {item['title'][:30]}...")
    
    def get_reddit_ai(self):
        try:
            url = "https://www.reddit.com/r/artificial/hot.json"
            headers = {"User-Agent": "WhatsAppAI/1.0"}
            response = requests.get(url, headers=headers)
            data = response.json()
            
            posts = []
            for post in data["data"]["children"][:5]:
                data = post["data"]
                posts.append({
                    "title": data["title"],
                    "content": data.get("selftext", "")[:200],
                    "url": f"https://reddit.com{data['permalink']}",
                    "upvotes": data["score"]
                })
            return posts
        except:
            return []
    
    def create_message(self, post):
        return f"""🚀 *AI Content Alert!*

📰 {post['title']}

{post['content']}...

🔗 {post['url']}

#AI #Viral #WhatsAppBot"""
    
    def save_to_file(self, message):
        with open("messages.txt", "a", encoding="utf-8") as f:
            f.write(f"\n{'='*50}\n")
            f.write(f"To: {self.number}\n")
            f.write(f"Time: {datetime.now().strftime('%H:%M')}\n")
            f.write(f"{message}\n")

if __name__ == "__main__":
    agent = WhatsAppAgent()
    agent.start()

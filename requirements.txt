import telebot # مكتبة التليجرام
import requests # مكتبة الطلبات

# 1. إعدادات البوت والتوكن
TOKEN = "8974893721:AAEmirealg6mKvZTYs9ijpP0lrOyhsJEibw"
bot = telebot.TeleBot(TOKEN)

# 2. دالة الفحص (المنطق البرمجي)
def check_username(username):
    username = username.replace("@", "").strip()
    url = f"https://www.instagram.com/{username}/"
    headers = {"User-Agent": "Mozilla/5.0"}
    try:
        req = requests.get(url, headers=headers, timeout=5)
        if req.status_code == 200:
            return "موجود ❌"
        return "متاح ✅"
    except:
        return "خطأ في الاتصال ⚠️"

# 3. معالجة أمر البداية
@bot.message_handler(commands=['start'])
def welcome(message):
    bot.reply_to(message, "أهلاً بك! أرسل يوزر إنستغرام للفحص.")

# 4. معالجة الرسائل العادية
@bot.message_handler(func=lambda message: True)
def process_message(message):
    text = message.text
    if text.startswith('/'): return
    
    # استدعاء دالة الفحص وإرسال النتيجة
    result = check_username(text)
    response = f"النتيجة لـ {text}:\nالحالة: {result}"
    bot.reply_to(message, response)

# 5. تشغيل البوت
if __name__ == "__main__":
    print("البوت يعمل الآن..")
    bot.polling()

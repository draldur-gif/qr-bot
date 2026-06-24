herefrom telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, filters, ContextTypes
import qrcode
import os

# ضع التوكن الجديد للبوت هنا
TOKEN = "8376552865:AAEMi3mGm2kR8diaffSDvFz5z9ZNyTU5r8M"

# أمر /start
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("📌 أرسل أي نص أو رابط وأنا أحوّله إلى QR Code")

# تحويل النص إلى QR Code وإرساله
async def make_qr(update: Update, context: ContextTypes.DEFAULT_TYPE):
    text = update.message.text
    img = qrcode.make(text)

    # مسار حفظ الصورة داخل مجلد التطبيق
    file_name = os.path.join(os.getcwd(), "qr.png")
    img.save(file_name)

    await update.message.reply_photo(photo=open(file_name, "rb"))
    os.remove(file_name)

# إنشاء التطبيق وإضافة الأوامر
app = ApplicationBuilder().token(TOKEN).build()
app.add_handler(CommandHandler("start", start))
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, make_qr))

print("Bot is running...")
app.run_polling()

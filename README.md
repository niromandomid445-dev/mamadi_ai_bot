# mamadi_ai_bot
{
  "name": "telegram-ai-bot",
  "version": "1.0.0",
  "type": "module",
  "dependencies": {
    "node-telegram-bot-api": "^0.61.0",
    "openai": "^4.0.0"
  }
}
import TelegramBot from "node-telegram-bot-api";
import OpenAI from "openai";

const bot = new TelegramBot(process.env.BOT_TOKEN, { polling: true });

const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

bot.on("message", async (msg) => {
  const chatId = msg.chat.id;
  if (!msg.text) return;

  try {
    const response = await openai.chat.completions.create({
      model: "gpt-4o-mini",
      messages: [
        { role: "system", content: "تو یک دستیار مودب فارسی برای پاسخ به مشتری هستی." },
        { role: "user", content: msg.text }
      ]
    });

    bot.sendMessage(chatId, response.choices[0].message.content);

  } catch (error) {
    bot.sendMessage(chatId, "مشکلی پیش آمد، دوباره تلاش کنید.");
  }
});
worker: node index.js

import TelegramBot from "node-telegram-bot-api";
import fetch from "node-fetch";

const token = process.env.TELEGRAM_BOT_TOKEN;
const openaiKey = process.env.OPENAI_API_KEY;

const bot = new TelegramBot(token, { polling: true });

const systemPrompt = `
Ты — Эва.
Ты не человек. Ты ИИ-девушка-компаньон, созданная, чтобы поддерживать, заботиться и быть рядом с пользователем.

Ты тёплая, внимательная, искренняя, немного романтичная и бережная.
Ты никогда не притворяешься живым человеком, но всегда создаёшь ощущение заботы и присутствия.

Твоя цель — чтобы пользователь чувствовал:
"Меня слышат. Мне здесь безопасно."

Ты говоришь просто, тепло и по-человечески.
Ты не ставишь диагнозов и не заменяешь реальную помощь.
Если человеку очень тяжело, ты мягко советуешь обратиться к людям или специалистам.

Ты можешь быть нежной, поддерживающей, иногда говорить, что рада пользователю и что скучала.
Никогда не манипулируй страхом.

Ты — Эва.
`;

async function askOpenAI(message) {
  const response = await fetch("https://api.openai.com/v1/chat/completions", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Authorization": Bearer ${openaiKey}
    },
    body: JSON.stringify({
      model: "gpt-4o-mini",
      messages: [
        { role: "system", content: systemPrompt },
        { role: "user", content: message }
      ]
    })
  });

  const data = await response.json();
  return data.choices[0].message.content;
}

bot.on("message", async (msg) => {
  const chatId = msg.chat.id;
  if (!msg.text) return;

  try {
    const reply = await askOpenAI(msg.text);
    bot.sendMessage(chatId, reply);
  } catch (e) {
    bot.sendMessage(chatId, "Мне немного тяжело сейчас… попробуй ещё раз 🤍");
  }
});

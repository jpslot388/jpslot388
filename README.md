const telegramAuthToken="7302179779:AAHRhyNPC0SviXQ6lUZsFaZh1YDsLz1aMlc";
const webhookEndpoint = "/endpoint";

addEventListener("fetch", event => {
  event.respondWith(handleIncomingRequest(event));
});
async function handleIncomingRequest(event) {
  const url = new URL(event.request.url);
  const path = url.pathname;
  const method = event.request.method;
  const workerUrl = ${url.protocol}//${url.host};
  
  if (method === "POST" && path === webhookEndpoint) {
    const update = await event.request.json();
    event.waitUntil(processUpdate(update));
   
    return new Response("Ok", { status: 200 });
  } else if (method === "GET" && path === "/configure-webhook") {
    const apiUrl = `http://t.me/Jpslot388_bot${telegramAuthToken}/setWebhook?url=${workerUrl}${webhookEndpoint}`;
    
    const response = await fetch(apiUrl);
    
    if (response.ok) {  
      return new Response("Webhook set successfully", { status: 200 });
    } else { 
      return new Response("Failed to set webhook", { status: response.status });
    }
  } else {
    return new Response("Not found", { status: 404 });
  }
}
async function processUpdate(update) {
  if ("message" in update) {
    const message = update.message;
    const chatId = message.chat.id;
    const text = message.text;

    if (text && text.startsWith("/start")) {
      const welcomeCaption = `
JPSLOT388 : adalah situs yang terbaik dan terpercaya diindonesia dengan menberikan layanan 𝟮𝟰/𝟳  
dengan deposit dan tarik dana dengan cepat dan aman

🎁 Welcome bonus situs JPSLOT388 50% diberikan di awal
      `;

      const photoUrl = "https://imgur.com/a/xveT8zx"; // Replace this with your actual photo URL

      const payload = {
        chat_id: chatId,
        photo: photoUrl,
        caption: welcomeCaption,
        parse_mode: "HTML", // or "Markdown" if you prefer
        reply_markup: {
          inline_keyboard: [
            [
              {
                text: "✏️ LInk daftar gacor",
                url: "https://t.me/Jpslot388_bot/Register"
              }
            ],
            [
              {
                text: "Login dan daftar link gacor",
                url: "https://t.me/Jpslot388_bot"
              }
            ]
          ]
        }
      };

      const sendPhotoUrl = `https://core.telegram.org/bots/api}/sendPhoto`;

      await fetch(sendPhotoUrl, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(payload)
      });
    }
  }
}

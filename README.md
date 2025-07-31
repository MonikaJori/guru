<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Business Assistant Chatbot</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #e0c3c3;
      margin: 0;
      padding: 0;
    }
    .container {
      max-width: 600px;
      margin: 40px auto;
      background: #ffffff4e;
      padding: 20px;
      box-shadow: 0 0 20px rgba(235, 235, 235, 0.1);
      border-radius: 8px;
    }
    h1 {
      text-align: center;
      color: #333;
    }
    .chat-box {
      max-height: 400px;
      overflow-y: auto;
      border: 1px solid #ccc;
      padding: 10px;
      margin-bottom: 10px;
      background: #fafafa;
    }
    .chat {
      margin-bottom: 10px;
    }
    .user {
      text-align: right;
      color: blue;
    }
    .bot {
      text-align: left;
      color: green;
    }
    input[type="text"] {
      width: calc(100% - 60px);
      padding: 10px;
      margin-right: 5px;
    }
    button {
      padding: 10px;
      margin: 5px 5px 5px 0;
      cursor: pointer;
    }
    .controls {
      text-align: center;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Business Assistant Chatbot</h1>
    <div class="chat-box" id="chat-box"></div>
    <div>
      <input type="text" id="user-input" placeholder="Ask your business question..." />
      <button onclick="sendMessage()">Send</button>
    </div>
    <div class="controls">
      <button onclick="toggleVoice()">🔊 Voice: <span id="voice-status">Off</span></button>
      <button onclick="clearChat()">🧹 Clear</button>
      <button onclick="downloadChat()">⬇️ Download</button>
      <button onclick="switchLanguage()">🌐 Language: <span id="lang-status">English</span></button>
    </div>
  </div>

  <script>
    const qaPairs = [
      { Q: "hii", A: "Hello how can I help you" },
      { Q: "what is your name", A: "I am Business chatbot assistant." },
      { Q: "name", A: "Business assistant chatbot" },
      { Q: "How can I grow my small business?", A: "Focus on quality service, build customer trust, and market effectively." },
      { Q: "What is the key to customer retention?", A: "Consistent service, engagement, and rewarding loyalty." },
      { Q: "How do I promote my business online?", A: "Use social media, digital ads, and SEO techniques." },
      { Q: "What is a growth strategy?", A: "A plan focused on increasing business revenue and market share." },
      { Q: "How can I increase customer engagement?", A: "Provide valuable content, offer discounts, and maintain regular communication." },
      { Q: "Why is branding important?", A: "Branding builds recognition, trust, and loyalty among customers." },
      { Q: "What is the best way to handle customer feedback?", A: "Listen actively, respond professionally, and improve based on feedback." },
      { Q: "How do I attract new clients?", A: "Offer competitive prices, quality service, and strong online presence." },
      { Q: "What role does networking play in business growth?", A: "Networking helps build relationships, partnerships, and potential client base." },
      { Q: "How do I set achievable business goals?", A: "Use SMART goals — Specific, Measurable, Achievable, Relevant, Time-bound." },
      { Q: "How can I leverage partnerships for growth?", A: "Form alliances with complementary businesses for mutual benefit." },
      { Q: "What is business diversification?", A: "Expanding into new products or markets to reduce risks." },
      { Q: "How do I manage business competition?", A: "By offering unique value propositions and understanding competitors." },
      { Q: "Why is customer service training important?", A: "It improves customer satisfaction and loyalty." },
      { Q: "What are business growth metrics?", A: "KPIs like revenue, profit, customer acquisition, and retention." },
      { Q: "How can I build an effective sales strategy?", A: "Identify target market, set goals, and train sales team." },
      { Q: "Why is customer onboarding important?", A: "It sets expectations and builds strong customer relationships." },
      { Q: "What is product differentiation?", A: "Creating unique product features to stand out in the market." },
      { Q: "How can I use testimonials for growth?", A: "Share positive customer reviews to build trust." },
      { Q: "What is a customer acquisition strategy?", A: "A plan for attracting and converting new customers." },
      { Q: "What is customer lifetime value?", A: "The total revenue a business earns from a single customer over time." },
      { Q: "Why is customer satisfaction important?", A: "It leads to repeat business and positive referrals." },
      { Q: "How can I create brand awareness?", A: "Through consistent messaging, marketing, and customer engagement." },
      { Q: "What is the importance of market analysis?", A: "It helps understand market demand and competition." },
      { Q: "How do I handle negative reviews?", A: "Respond politely, address concerns, and improve services." },
      { Q: "What is customer engagement?", A: "Building a relationship with customers through meaningful interactions." },
      { Q: "Why is feedback important for business?", A: "It helps identify areas of improvement and builds customer loyalty." },
      { Q: "How do I measure business performance?", A: "Using KPIs like revenue, profit margins, and customer retention." },
      { Q: "What is business innovation?", A: "Introducing new ideas, services, or products to improve business." },
      { Q: "How can I make my business unique?", A: "Offer something different, focus on customer needs, and innovate." },
      { Q: "What is digital transformation?", A: "Integrating digital technology into all areas of business." },
      { Q: "How can I use social media for growth?", A: "Engage customers, run ads, and build community online." },
      { Q: "What is inbound marketing?", A: "Attracting customers through valuable content and experiences." },
      { Q: "Why is business ethics important?", A: "It builds trust, reputation, and long-term success." },
      { Q: "How can I improve customer service?", A: "Train staff, listen to feedback, and solve issues quickly." },
      { Q: "What is an entrepreneur mindset?", A: "A mindset focused on innovation, risk-taking, and growth." },
      { Q: "Why should I analyze customer behavior?", A: "To tailor marketing and improve customer satisfaction." },
      { Q: "How do I identify new market opportunities?", A: "Through market research and customer feedback." },
      { Q: "What is a competitive advantage?", A: "A factor that allows you to outperform competitors." },
      { Q: "Why is networking important in business?", A: "It opens doors to new opportunities and collaborations." },
      { Q: "How can I motivate my employees?", A: "By recognizing achievements and providing growth opportunities." },
      { Q: "What is a sales funnel?", A: "The journey a customer takes from awareness to purchase." },
      { Q: "Why invest in customer experience?", A: "It increases satisfaction, loyalty, and business growth." },
      { Q: "How can I enhance brand loyalty?", A: "Deliver consistently great products and excellent service." },
      { Q: "What is strategic planning?", A: "Setting goals and defining the direction for business success." },
      { Q: "How do I reduce business risk?", A: "With careful planning, diversification, and risk management." },
      { Q: "Why is business communication key?", A: "It ensures clarity, builds relationships, and drives results." },
      { Q: "How do I handle business failure?", A: "Learn from mistakes, adapt, and keep moving forward." },
      { Q: "What is business sustainability?", A: "Operating in ways that protect the environment and society." },
      { Q: "Why set clear business objectives?", A: "They guide actions and measure success effectively." },
      { Q: "What is cross-selling?", A: "Selling additional products related to the original purchase." },
      { Q: "How do I create a business pitch?", A: "Keep it clear, concise, and focused on benefits." },
      { Q: "What is upselling?", A: "Encouraging customers to buy a higher-end product." },
      { Q: "How do I manage business cash flow?", A: "Monitor income and expenses regularly." },
      { Q: "Why is employee engagement important?", A: "It boosts productivity, satisfaction, and retention." },
      { Q: "How do I develop a marketing strategy?", A: "Understand market, define goals, and select tactics." },
      { Q: "What is affiliate marketing?", A: "Earning commission by promoting other companies' products." },
      { Q: "How do I build a business brand?", A: "Define identity, values, and ensure consistent messaging." },
      { Q: "Why should I track competitors?", A: "To stay ahead and adapt your strategy." },
      { Q: "What is influencer marketing?", A: "Using influencers to promote products to their audience." },
      { Q: "How can I improve business productivity?", A: "Streamline processes, invest in tools, and train staff." },
      { Q: "What is a unique selling proposition?", A: "What makes your business different and better." },
      { Q: "Why is teamwork vital in business?", A: "It fosters collaboration and improves performance." },
      { Q: "How do I handle customer objections?", A: "Listen, empathize, and offer solutions." },
      { Q: "What is business automation?", A: "Using technology to perform tasks with minimal human input." },
      { Q: "Why conduct customer surveys?", A: "To gather feedback and improve services." },
      { Q: "How do I scale a startup?", A: "Secure funding, build systems, and grow customer base." },
      { Q: "What is brand consistency?", A: "Maintaining the same voice and message across all platforms." },
      { Q: "Why is continuous learning important?", A: "To adapt to changes and remain competitive." },
      { Q: "What is customer journey mapping?", A: "Visualizing steps customers take with your business." },
      { Q: "How do I stay motivated in business?", A: "Set goals, celebrate wins, and stay passionate." },
      { Q: "What is business resilience?", A: "The ability to recover from setbacks and adapt to change." },
      { Q: "How do I create a marketing plan?", A: "Research, set objectives, and outline tactics." },
      { Q: "Why focus on customer needs?", A: "It builds loyalty and drives long-term growth." }
    ];

    let voiceEnabled = false;
    let language = "en";

    function sendMessage() {
      const input = document.getElementById("user-input");
      const message = input.value.trim();
      if (message === "") return;
      appendMessage("user", message);
      input.value = "";

      const response = getResponse(message);
      appendMessage("bot", response);
      if (voiceEnabled) speak(response);
    }

    function appendMessage(sender, message) {
      const chatBox = document.getElementById("chat-box");
      const msgDiv = document.createElement("div");
      msgDiv.className = `chat ${sender}`;
      msgDiv.innerText = message;
      chatBox.appendChild(msgDiv);
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function getResponse(question) {
      const input = question.toLowerCase();
      let bestMatch = null;
      let maxMatchScore = 0;

      qaPairs.forEach(pair => {
        const q = pair.Q.toLowerCase();
        let score = 0;
        q.split(" ").forEach(word => {
          if (input.includes(word)) score++;
        });
        if (score > maxMatchScore) {
          maxMatchScore = score;
          bestMatch = pair;
        }
      });

      return maxMatchScore > 0 ? bestMatch.A : "Sorry, I don't have an answer to that question.";
    }

    function toggleVoice() {
      voiceEnabled = !voiceEnabled;
      document.getElementById("voice-status").innerText = voiceEnabled ? "On" : "Off";
    }

    function speak(text) {
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = language === "en" ? "en-US" : "hi-IN";
      speechSynthesis.speak(utterance);
    }

    function clearChat() {
      document.getElementById("chat-box").innerHTML = "";
    }

    function downloadChat() {
      const chatBox = document.getElementById("chat-box");
      let content = "";
      chatBox.querySelectorAll(".chat").forEach(div => {
        content += div.classList.contains("user") ? "You: " : "Bot: ";
        content += div.innerText + "\n";
      });
      const blob = new Blob([content], { type: "text/plain" });
      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");
      link.href = url;
      link.download = "chat_history.txt";
      link.click();
      URL.revokeObjectURL(url);
    }

    function switchLanguage() {
      language = language === "en" ? "hi" : "en";
      document.getElementById("lang-status").innerText = language === "en" ? "English" : "Hindi";
    }
  </script>
</body>
</html>

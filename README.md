<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>72nd BPSC - बजट 2026-27 क्विज़</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen p-4 flex flex-col items-center">
  <div class="max-w-2xl w-full bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-xl my-auto">
    <div class="flex justify-between items-center mb-4">
      <span id="tracker" class="text-xs text-indigo-400 font-bold bg-indigo-950/60 px-3 py-1 rounded-full border border-indigo-800">प्रश्न 1 / 20</span>
      <span id="score" class="text-xs text-emerald-400 font-bold bg-emerald-950/60 px-3 py-1 rounded-full border border-emerald-800">अंक: 0.00</span>
    </div>
    
    <h2 id="question-text" class="text-base font-semibold text-slate-100 mb-6 leading-relaxed"></h2>
    
    <div id="options-box" class="space-y-3"></div>
    
    <div id="feedback" class="hidden mt-5 p-4 rounded-xl text-sm border"></div>
    
    <div class="flex justify-between mt-6 pt-4 border-t border-slate-800">
      <button id="prev-btn" onclick="prevQ()" class="px-4 py-2 bg-slate-800 text-slate-300 rounded-xl text-sm font-medium border border-slate-700 disabled:opacity-30">← पिछला</button>
      <button id="next-btn" onclick="nextQ()" class="px-6 py-2 bg-indigo-600 text-white rounded-xl text-sm font-medium shadow-md shadow-indigo-600/30">अगला →</button>
    </div>
  </div>

  <script>
    const quizData = [
      { q: "केंद्रीय बजट 2026-27 का मुख्य विषय (Theme) क्या रखा गया है?", opt: ["विकसित भारत 2047", "युवा शक्ति-संचालित बजट", "आत्मनिर्भर एवं सक्षम भारत", "समग्र विकास एवं हरित ऊर्जा", "प्रयास नहीं किया गया"], ans: 1, exp: "बजट 2026-27 का मुख्य विषय 'युवा शक्ति-संचालित बजट' है।" },
      { q: "केंद्रीय बजट 2026-27 के अनुसार कुल बजट आकार कितना है?", opt: ["₹48,20,512 करोड़", "₹53,47,315 करोड़", "₹55,12,821 करोड़", "₹59,14,165 करोड़", "प्रयास नहीं किया गया"], ans: 1, exp: "कुल बजट आकार ₹53,47,315 करोड़ (लगभग ₹53.5 लाख करोड़) अनुमानित है।" },
      { q: "बजट 2026-27 के अनुसार राजकोषीय घाटा (Fiscal Deficit) GDP का कितना प्रतिशत है?", opt: ["4.1%", "4.3%", "4.5%", "4.8%", "प्रयास नहीं किया गया"], ans: 1, exp: "राजकोषीय घाटा GDP का 4.3% (₹16,95,762 करोड़) अनुमानित है।" },
      { q: "बजट 2026-27 में कौन-सा घाटा GDP का 0.3% रहने का अनुमान है?", opt: ["राजकोषीय घाटा", "राजस्व घाटा", "प्रभावी राजस्व घाटा", "प्राथमिक घाटा", "प्रयास नहीं किया गया"], ans: 2, exp: "प्रभावी राजस्व घाटा GDP का 0.3% अनुमानित है।" },
      { q: "'रुपया कहाँ से आता है' के अनुसार सरकारी प्राप्तियों की सबसे बड़ी मद कौन-सी है?", opt: ["आयकर (21%)", "निगम कर (18%)", "उधार एवं अन्य देयताएं (24%)", "GST (15%)", "प्रयास नहीं किया गया"], ans: 2, exp: "उधार एवं अन्य देयताएं (24%) सबसे बड़ी मद है।" },
      { q: "'रुपया कहाँ जाता है' के अनुसार व्यय की सबसे बड़ी मद कौन-सी है?", opt: ["ब्याज अदायगी (20%)", "रक्षा व्यय (11%)", "केंद्रीय क्षेत्र योजनाएं (17%)", "करों व शुल्कों में राज्यों का हिस्सा (22%)", "प्रयास नहीं किया गया"], ans: 3, exp: "करों और शुल्कों में राज्यों का हिस्सा (22%) सबसे बड़ी मद है।" },
      { q: "'बायोफार्मा शक्ति' पहल हेतु अगले पाँच वर्षों के लिए कितना आवंटन प्रस्तावित है?", opt: ["₹5,000 करोड़", "₹10,000 करोड़", "₹15,000 करोड़", "₹20,000 करोड़", "प्रयास नहीं किया गया"], ans: 1, exp: "बायोफार्मा शक्ति हेतु ₹10,000 करोड़ का प्रावधान है।" },
      { q: "इलेक्ट्रॉनिक्स कलपुर्जे विनिर्माण योजना का बजट बढ़ाकर कितना किया गया है?", opt: ["₹20,000 करोड़", "₹30,000 करोड़", "₹40,000 करोड़", "₹50,000 करोड़", "प्रयास नहीं किया गया"], ans: 2, exp: "इलेक्ट्रॉनिक्स कलपुर्जे विनिर्माण का बजट ₹40,000 करोड़ किया गया है।" },
      { q: "दुर्लभ धातु गलियारे (Rare Earth Corridors) किन राज्यों की मदद से बनेंगे?", opt: ["ओडिशा, केरल, आंध्र प्रदेश और तमिलनाडु", "झारखंड, ओडिशा, छत्तीसगढ़ और MP", "राजस्थान, गुजरात, महाराष्ट्र और कर्नाटक", "असम, मेघालय, प. बंगाल और बिहार", "प्रयास नहीं किया गया"], ans: 0, exp: "खनिज समृद्ध ओडिशा, केरल, आंध्र प्रदेश और तमिलनाडु में बनेंगे।" },
      { q: "केमिकल उत्पादन बढ़ाने हेतु किस मॉडल पर 3 केमिकल पार्क बनेंगे?", opt: ["BOT मॉडल", "बनाओ और चलाओ (Build and Operate)", "HAM मॉडल", "EPC मॉडल", "प्रयास नहीं किया गया"], ans: 1, exp: "'बनाओ और चलाओ' मॉडल पर तीन समर्पित केमिकल पार्क बनेंगे।" },
      { q: "वस्त्र क्षेत्र में आत्मनिर्भरता हेतु कौन-सी नई योजना लाई गई है?", opt: ["राष्ट्रीय फाइबर योजना", "पीएम मित्र योजना", "समर्थ 2.0 मिशन", "खादी ग्राम स्वराज", "प्रयास नहीं किया गया"], ans: 0, exp: "रेशम, ऊन, जूट व मानव निर्मित फाइबर हेतु राष्ट्रीय फाइबर योजना घोषित है।" },
      { q: "MSME को चैंपियन बनाने हेतु 'MSME ग्रोथ फंड' कितने आवंटन से शुरू होगा?", opt: ["₹5,000 करोड़", "₹10,000 करोड़", "₹15,000 करोड़", "₹20,000 करोड़", "प्रयास नहीं किया गया"], ans: 1, exp: "MSME ग्रोथ फंड के लिए ₹10,000 करोड़ आवंटित किए गए हैं।" },
      { q: "वित्त वर्ष 2026-27 में सार्वजनिक पूँजीगत व्यय (Capex) कितना प्रस्तावित है?", opt: ["₹10.5 लाख करोड़", "₹11.2 लाख करोड़", "₹12.2 लाख करोड़", "₹13.1 लाख करोड़", "प्रयास नहीं किया गया"], ans: 2, exp: "सार्वजनिक पूँजीगत व्यय ₹12.2 लाख करोड़ प्रस्तावित है।" },
      { q: "दानकुनी (प. बंगाल) से सूरत (गुजरात) DFC कॉरिडोर की लंबाई कितनी है?", opt: ["1,500 किमी", "1,800 किमी", "2,100 किमी", "2,400 किमी", "प्रयास नहीं किया गया"], ans: 2, exp: "दानकुनी से सूरत तक का कॉरिडोर लगभग 2,100 किमी लंबा है।" },
      { q: "NW-5 ओडिशा के खनिज क्षेत्रों को किन बंदरगाहों से जोड़ेगा?", opt: ["हल्दिया और विशाखापत्तनम", "पारादीप और धामरा", "कोच्चि और मंगलुरु", "मुंबई और कांडला", "प्रयास नहीं किया गया"], ans: 1, exp: "NW-5 तालचेर-अनुगुल को पारादीप और धामरा बंदरगाहों से जोड़ेगा।" },
      { q: "तटीय नौवहन की हिस्सेदारी को 2047 तक कितना करने का लक्ष्य है?", opt: ["8%", "10%", "12%", "15%", "प्रयास नहीं किया गया"], ans: 2, exp: "तटीय नौवहन हिस्सेदारी को 6% से बढ़ाकर 12% करने का लक्ष्य है।" },
      { q: "CCUS प्रौद्योगिकी के विकास हेतु 5 वर्षों में कितनी राशि का प्रावधान है?", opt: ["₹10,000 करोड़", "₹15,000 करोड़", "₹20,000 करोड़", "₹25,000 करोड़", "प्रयास नहीं किया गया"], ans: 2, exp: "CCUS हेतु ₹20,000 करोड़ आवंटित किए गए हैं।" },
      { q: "प्रत्येक शहर आर्थिक क्षेत्र (CER) हेतु 5 वर्षों के लिए कितना फंड तय है?", opt: ["₹2,000 करोड़", "₹3,000 करोड़", "₹5,000 करोड़", "₹10,000 करोड़", "प्रयास नहीं किया गया"], ans: 2, exp: "प्रत्येक CER के विकास हेतु ₹5,000 करोड़ का प्रावधान है।" },
      { q: "₹1,000 करोड़ से अधिक का म्युनिसिपल बॉन्ड जारी करने पर कितना प्रोत्साहन मिलेगा?", opt: ["₹50 करोड़", "₹100 करोड़", "₹150 करोड़", "₹200 करोड़", "प्रयास नहीं किया गया"], ans: 1, exp: "सिंगल बॉन्ड पर ₹100 करोड़ की प्रोत्साहन राशि का प्रावधान है।" },
      { q: "ऑरेंज इकोनॉमी हेतु 15,000 स्कूलों और 500 कॉलेजों में क्या बनेगा?", opt: ["फिल्म हब", "AVGC कंटेंट क्रिएटर लैब", "डिजाइन सेंटर", "रोबोटिक्स लैब", "प्रयास नहीं किया गया"], ans: 1, exp: "AVGC कंटेंट क्रिएटर लैब स्थापित की जाएगी।" }
    ];

    let cur = 0;
    let userAns = new Array(quizData.length).fill(null);

    function getScore() {
      let s = 0;
      userAns.forEach((a, i) => {
        if (a === null || a === 4) return;
        if (a === quizData[i].ans) s += 1;
        else s -= 0.33;
      });
      return Math.max(0, s).toFixed(2);
    }

    function render() {
      const item = quizData[cur];
      document.getElementById("tracker").innerText = `प्रश्न ${cur + 1} / ${quizData.length}`;
      document.getElementById("score").innerText = `अंक: ${getScore()}`;
      document.getElementById("question-text").innerText = item.q;
      document.getElementById("prev-btn").disabled = cur === 0;
      document.getElementById("next-btn").innerText = cur === quizData.length - 1 ? "पूर्ण" : "अगला →";

      const optBox = document.getElementById("options-box");
      const feed = document.getElementById("feedback");
      optBox.innerHTML = "";
      feed.className = "hidden mt-5 p-4 rounded-xl text-sm border";

      const sel = userAns[cur];
      const tags = ["(A)", "(B)", "(C)", "(D)", "(E)"];

      item.opt.forEach((o, i) => {
        const btn = document.createElement("button");
        btn.className = "w-full text-left p-3 rounded-xl text-sm transition border flex gap-2.5 ";
        if (sel === null) {
          btn.className += "bg-slate-800/80 hover:bg-slate-800 text-slate-200 border-slate-700";
        } else {
          if (i === item.ans) btn.className += "bg-emerald-950/60 text-emerald-300 border-emerald-500 font-bold";
          else if (sel === i) btn.className += i === 4 ? "bg-amber-950/60 text-amber-300 border-amber-500" : "bg-rose-950/60 text-rose-300 border-rose-500";
          else btn.className += "bg-slate-900 text-slate-600 border-slate-800 opacity-50";
        }
        btn.innerHTML = `<span class="font-bold">${tags[i]}</span> <span>${o}</span>`;
        btn.onclick = () => { if (userAns[cur] === null) { userAns[cur] = i; render(); } };
        optBox.appendChild(btn);
      });

      if (sel !== null) {
        feed.classList.remove("hidden");
        if (sel === 4) {
          feed.className = "mt-5 p-4 rounded-xl text-sm border bg-amber-950/40 border-amber-500 text-amber-200";
          feed.innerHTML = `<b>⚠️ प्रयास नहीं किया गया (0 अंक)</b><div class="mt-1 text-slate-300"><b>सही उत्तर:</b> ${tags[item.ans]} - ${item.opt[item.ans]}</div><div class="text-xs text-slate-400 mt-1"><b>व्याख्या:</b> ${item.exp}</div>`;
        } else if (sel === item.ans) {
          feed.className = "mt-5 p-4 rounded-xl text-sm border bg-emerald-950/40 border-emerald-500 text-emerald-200";
          feed.innerHTML = `<b>✓ सही उत्तर (+1.00 अंक)</b><div class="text-xs text-slate-300 mt-1"><b>व्याख्या:</b> ${item.exp}</div>`;
        } else {
          feed.className = "mt-5 p-4 rounded-xl text-sm border bg-rose-950/40 border-rose-500 text-rose-200";
          feed.innerHTML = `<b>✗ गलत उत्तर (-0.33 अंक दंड)</b><div class="mt-1 text-slate-300"><b>सही उत्तर:</b> ${tags[item.ans]} - ${item.opt[item.ans]}</div><div class="text-xs text-slate-400 mt-1"><b>व्याख्या:</b> ${item.exp}</div>`;
        }
      }
    }

    function prevQ() { if (cur > 0) { cur--; render(); } }
    function nextQ() { if (cur < quizData.length - 1) { cur++; render(); } }
    render();
  </script>
</body>
</html>

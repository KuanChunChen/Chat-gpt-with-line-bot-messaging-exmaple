# [2023][ChatGPT][Open AI][Kotlin][懶人包教學]一步一步帶你把ChatGPT串進你的LineBot聊天室

<h2 style="background-color:MediumSeaGreen; color:white;">前導：和ChatGPT聊聊天吧</h2>

<div align="center">
  <h3>1.註冊帳號，點擊Sign Up註冊:
    <a href="https://chat.openai.com/auth/login">ChatGPT 登入頁面</a>
  </h3>

  <img src="/images/linebot/line_bot_0000.png" width="60%"/><br><br>
  <h3>2.建立你的Account</h3>

  <img src="/images/linebot/line_bot_00000.png" width="45%"/><br><br>
  <h3>3.開始聊聊天，在下面對話框輸入你要問的問題</h3>

  <img src="/images/linebot/line_bot_009.png" width="70%"/><br><br>
  <h3>4.像是...2023年WBC經典賽冠軍預測</h3>

  <img src="/images/linebot/line_bot_010.png" width="70%"/><br><br>
  <h3>5.或是...怎麼用Kotlin寫一個預測的程式呢？</h3>
  <img src="/images/linebot/line_bot_011.png" width="70%"/>
  <img src="/images/linebot/line_bot_012.png" width="70%"/>
  <img src="/images/linebot/line_bot_013.png" width="70%"/>
  <p>&#11014;看起來chatGPT給了一段給身高預測體重的範例，看起來有模有樣</p>

</div>

<h5>這個那麼厲害的AI我們都知道能夠問他千奇百怪的問題，那要怎麼為你所用呢？我們接著看下去...</h5>


<h2 style="background-color:MediumSeaGreen; color:white;">試著串接ChatGPT API吧</h2>

<div align="center">
  <h3>1.註冊一個賬號並獲取API keys：
    <a href="https://platform.openai.com/account/api-keys">OpenAI 登入頁面</a>
  </h3>

  <img src="/images/linebot/line_bot_014.png" width="30%"/><br>
  <p>&#11014;點擊進入後右上角 頭像點進後會有如上圖樣式，點擊View API keys即可</p>
  <img src="/images/linebot/line_bot_015.png" width="60%"/><br><br>
  <p>&#11014;點擊Create new Security key，這個Key是你之後呼叫API使用要確認你身份的一把Key</p>

  <h3>2.接著你可以看官方api文件：
    <a href="https://platform.openai.com/docs/api-reference/models/list">OpenAI api文件</a>
  </h3>
  <p>看文件介紹怎麼串，再照文件上說明去串</p>

  <img src="/images/linebot/line_bot_017.png" width="45%"/><br><br>
  <p>不過若是對curl或api請求稍微有經驗了，可以直接找到官方提供的curl範例，去改成你熟悉的語言請求</p>
  <img src="/images/linebot/line_bot_016.png" width="45%"/><br><br>
  <pre style="text-align: left;">
  <code>
  curl https://api.openai.com/v1/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -d '{"model": "text-davinci-003", "prompt": "Say this is a test", "temperature": 0, "max_tokens": 7}'
  </code>
  </pre>
    <p style="text-align:left;">
    &#11014; 快速介紹一下上面這段curl的含義<br>
    1. 我們要發送request的url是https://api.openai.com/v1/completions<br>
    2. 如果要拆成更細可以看成前段https://api.openai.com/ domain name<br>
    跟後段API接口 v1/completions<br>
    3. 中間-H的部分是Header<br>
    Content-Type: application/json主要用途為我們request body的格式要為json<br>
    Authorization: Bearer $YOUR_API_KEY 這個則是你要使用OpenAI提供的API你必需要輸入一個驗證API key<br>
    也就是我們前面產生的key<br>
    4. -d '{....}' 最後面-d是要傳給接口的json格式，{}框框內即要傳的json內容<br>
    5. 簡單json key解釋：<br>
    model ：為chatGPT的模型，官方有提供不同種的模型供串接者使用，<br>
    每個都有最大token，或是收費，甚至可靠性不一，可以自己根據文件來測試：
    <a href="https://platform.openai.com/docs/models/gpt-3">GPT-3 model文件</a><br><br>
    prompt：就是你要問的問題，跟前面你直接輸入到chatGPT網頁版的聊天室一樣，<br>
    只是現在變成你自己用程式去發請求<br>
    max_tokens： 則是你想要這次請求最多可以幾個tokens限制，<br>
    因為官方應該是用tokens數字來收費，<br>
    所以可能可以透過max_tokens來限制，<br>
    每次的請求，可能是讓有長期規劃使用該api的人可以控制流量吧?<br><br>
    （這邊的tokens只是官方用來計算流量計費的一種方式，並非常見用token來驗證的那種token）
    </p>

  <h3>3.至此，你已經取得串接OpenAI接口所需的東西了...</h3>
  <p>可以開始使用你熟悉的語言來開發API了</p>
  <p>串接 OpenAI API 的 Kotlin 程式</p>

  <script src="https://gist.github.com/KuanChunChen/4bcd72e0ba21a76eb545112113be7cfa.js"></script>

  <p style="text-align:left;">
  &#11014; 這邊我習慣把各種有可能會覆用的code拉出來寫，ChatGptAPI.kt、ChatGptCompletionRequest.kt、ChatGptCompletionResult.kt...等等<br>
  中間因為沒有要寫太大的專案，就懶得自己寫thread操作了<br>
  先用最簡單用的retrofit內建Callback<br>
  裡面已經幫忙處理UI Thread跟sub Thread的切換了
  </p>

  <script src="https://gist.github.com/KuanChunChen/04e812ff6d93a27e1ba8a91834b2f140.js"></script>
  <p style="text-align:left;">
  &#11014; 這邊主要是用Retrofit把串接接口分離出來
  </p>

  <script src="https://gist.github.com/KuanChunChen/a4b7da41bfe56c408b879fdc8ceac03b.js"></script>
  <p style="text-align:left;">
  &#11014; 這邊就是建立一個http連線的類</p>

  <h3>4.完成上面你就已經成功串接ChatGpt的API啦~</h3>
  <p>現在你只需要再在你呼叫ChatGpt API成功的地方<br>
  去呼叫LineBot聊天室的API就能把返回的消息傳到你實際在使用的Line聊天室內了</p>
</div>



<h2 style="background-color:MediumSeaGreen; color:white;">開始建立LineBot帳號</h2>

<h3>1.
申請Line Bot賬號：首先需要到Line Bot開發者中心申請一個Line Bot賬號，並創建一個新的Line Bot Channel。</h3>

點此連結去申請或直接用line帳號登入：[Line Business ID](https://account.line.biz/login?redirectUri=https%3A%2F%2Fdevelopers.line.biz%2Fconsole%2Fchannel%2F1656655880%2Fmessaging-api)

<div align="center">
  <img src="/images/linebot/line_bot_001.png" width="45%"/>
  <img src="/images/linebot/line_bot_002.png" width="45%"/>
</div><br>

<h3>
2.
配置Line Bot Channel：創建Line Bot Channel後，需要配置Channel基本信息、Webhook、消息API、Line Login等功能。</h3>

註冊完後，進入此畫面，點擊Create創建新的聊天室：<br>
<div align="center">
  <img src="/images/linebot/line_bot_003.png" width="50%"/>
  <img src="/images/linebot/line_bot_004.png" width="40%"/>
</div><br>

創建後，來到這個頁面，點擊Create a Messaging API Channel 來開通使用line bot的訊息API：<br>

<div align="center">
  <img src="/images/linebot/line_bot_005.png" width="100%"/>
</div><br>

依照下圖，輸入資料<br>

<div align="center">
  <img src="/images/linebot/line_bot_006.png" width="100%"/>
</div><br>
<div align="center">
  <img src="/images/linebot/line_bot_007.png" width="100%"/>
</div><br>

最後輸入完後<br>
記得在條約打勾後創建<br>

<div align="center">
  <img src="/images/linebot/line_bot_008.png" width="100%"/>
</div><br>

<h3>
3.
創建完後可以分別在Basic Setting 跟 Messaging API頁面看到你的Channel secret 與Channel access token
</h3>
這邊兩組key是呼叫linebot相關接口需要的key
<div align="center">
  <img src="/images/linebot/line_bot_018.png" width="50%"/><br><br>
  <img src="/images/linebot/line_bot_019.png" width="80%"/>
</div><br>

<h3>4.接著就是參考參考LineBot官方API文件，看看怎麼串:<a href="https://developers.line.biz/en/docs/messaging-api/sending-messages/#methods-of-sending-message">LineBot Messaging api文件</a></h3>
<div align="center">
  <img src="/images/linebot/line_bot_020.png" width="50%"/><br><br>
</div><br>

<h3>5.至此，你已經取得串接LineBot接口所需的東西了...</h3>
<p>可以開始使用你熟悉的語言來開發API了</p>
<p>串接 LineBot API 的 Kotlin 程式</p>
<script src="https://gist.github.com/KuanChunChen/a21b726e6cde1d2f171ca77b66b78abb.js"></script>
<p style="text-align:left;">
&#11014; 這裡跟前面ChatGPT串接的過程一樣，也是使用Retrofit來寫
</p>

<script src="https://gist.github.com/KuanChunChen/371d803d654c0050574da73df02d3f16.js"></script>
<p style="text-align:left;">
&#11014; 拉出來的Line Messaging接口
</p>

<h3>6.到這邊就簡單串完了..可以開始部署代碼到Server上了</h3>

可以用一些雲端Server或在自己本地IP架設Server把寫好的代碼放上去<br>
即可開始你的LineBot串接ChatGpt服務<br>
後面則是反覆測試你上線的功能是否有bug、後續維護都是可以注意的地方
剩下就自行去摸索吧，快來試試看！<br>

<h3 align="center">最終成果</h3>
<div align="center">
  <img src="/images/linebot/line_bot_021.png" width="40%"/><br><br>
</div><br>

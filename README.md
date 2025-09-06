<!-- <!DOCTYPE html> -->
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Lecture 1: Introduction to Python — Flipbook</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    :root{--bg:#0b1220;--card:#121a2a;--ink:#e6eefc;--muted:#96a2be;--accent:#2dd4bf;--accent2:#60a5fa}
    html,body{height:100%;background:radial-gradient(1200px 800px at 20% -10%, #0f1a33 0%, #0b1220 45%, #070c16 100%);} 
    .app{font-family:Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial; color:var(--ink)}
    .deck{ perspective: 1400px; }
    .page{ transform-style: preserve-3d; transition: transform 600ms cubic-bezier(.2,.8,.2,1), box-shadow 600ms; }
    .page.enter{ transform: rotateY(-12deg) translateZ(0px); box-shadow: 0 40px 80px rgba(0,0,0,.35)}
    .page.center{ transform: rotateY(0deg) translateZ(8px); box-shadow: 0 50px 120px rgba(0,0,0,.45)}
    .page.exit{ transform: rotateY(12deg) translateZ(0px); box-shadow: 0 40px 80px rgba(0,0,0,.35)}
    .code{ font-family: "JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; }
    .kbd{ font-family: "JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; border:1px solid rgba(255,255,255,.15); padding:2px 6px; border-radius:.375rem; }
    .dot{ width:.5rem;height:.5rem;border-radius:9999px;display:inline-block }
    .num-badge{background:linear-gradient(135deg,var(--accent2),var(--accent));}
    .btn{ backdrop-filter: blur(6px); background: rgba(255,255,255,0.06); border:1px solid rgba(255,255,255,.12)}
    .btn:hover{ background: rgba(255,255,255,0.12)}
    .shadow-soft{ box-shadow: 0 20px 60px rgba(0,0,0,.35) }
    .grid-auto{ display:grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap:1rem }
    /* Thank You Slide Styling */

.thankyou-slide {
    display: flex;
    justify-content: space-between; /* text left, image right */
    align-items: center;
    background: linear-gradient(135deg, #0d2543 031636, #0d2543);
    color: white;
    padding: 20px;
    position: relative;
    overflow: hidden;
  }

  .thankyou-content {
    flex: 1;
    text-align: left;
    z-index: 2;
  }

  .thankyou-text {
    font-size: 2.5em;
    font-weight: bold;
    animation: fadeInZoom 2s ease forwards;
  }

  .thankyou-sub {
    font-size: 1.2em;
    margin-top: 10px;
    opacity: 0;
    animation: fadeIn 3s ease forwards;
    animation-delay: 1.5s;
  }

  .thankyou-image {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2;
    height: 500px;
  }

  .thankyou-image img {
    /* max-width: 80%;
    max-height: 250px; */
    /* border-radius: 12px; */
    /* box-shadow: 0 6px 12px rgba(0,0,0,0.25); */
    animation: floatImg 4s ease-in-out infinite;
  }

  /* Animation Effects */
  @keyframes fadeInZoom {
    0% { opacity: 0; transform: scale(0.7); }
    100% { opacity: 1; transform: scale(1); }
  }

  @keyframes fadeIn {
    0% { opacity: 0; }
    100% { opacity: 1; }
  }

  @keyframes floatImg {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-12px); }
  }

  /* Confetti canvas styling */
  #confetti-canvas {
    position: absolute;
    top: 0; left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 1;
  }


  </style>
</head>
<body class="app">
  <div class="min-h-screen flex flex-col">
    <!-- Top Bar -->
    <header class="sticky top-0 z-40">
      <div class="mx-auto max-w-6xl px-4 py-4 flex items-center justify-between">
        <div class="flex items-center gap-3">
          <div class="w-9 h-9 rounded-2xl grid place-items-center bg-gradient-to-br from-sky-500 to-teal-300 text-slate-900 text-xl">🐍</div>
          <div class="">
            <h1 class="text-lg font-semibold">Lecture 1: Introduction to Python</h1>
            <p class="text-xs text-[var(--muted)]">Flipbook slides — use ←/→ keys or buttons</p>
          </div>
        </div>
        <div class="flex items-center gap-2">
          <span class="text-sm text-[var(--muted)]">Slide</span>
          <span id="pageNum" class="text-sm font-semibold"></span>
          <span class="text-sm text-[var(--muted)]">/</span>
          <span id="pageTotal" class="text-sm"></span>
        </div>
      </div>
    </header>

    <!-- Deck -->
    <main class="flex-1">
      <div class="deck max-w-5xl mx-auto p-4">
        <div id="page" class="page center rounded-3xl border border-white/10 bg-[var(--card)] shadow-soft overflow-hidden">
          <!-- Slide container -->
          <section id="slides" class="min-h-[68vh] flex items-stretch"></section>
        </div>

        <!-- Controls -->
        <div class="mt-6 flex items-center justify-between">
          <div class="flex items-center gap-3 text-[var(--muted)] text-xs">
            <span class="dot bg-sky-400"></span> concepts
            <span class="dot bg-emerald-400"></span> examples
            <span class="dot bg-fuchsia-400"></span> tasks
          </div>
          <div class="flex gap-3">
            <button id="prev" class="btn rounded-2xl px-4 py-2 text-sm">← Prev</button>
            <button id="next" class="btn rounded-2xl px-4 py-2 text-sm">Next →</button>
          </div>
        </div>

        <!-- Tips -->
        <p class="mt-4 text-center text-xs text-[var(--muted)]">Tip: You can also click the left/right side of the slide or use your keyboard arrows.</p>
      </div>
    </main>

    <footer class="mt-10 py-6 text-center text-[10px] text-[var(--muted)]">
      © 2025 • Python Lecture 1 Flipbook
    </footer>
  </div>

  <!-- Templates for slides -->
  <template id="tpl">
    <!-- Cover -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="text-center max-w-3xl">
        <div class="mx-auto w-24 h-24 rounded-3xl grid place-items-center text-6xl bg-gradient-to-br from-sky-500 to-teal-300 text-slate-900">🐍</div>
        <h2 class="mt-6 text-4xl sm:text-5xl font-extrabold tracking-tight">Lecture 1: Introduction to Python Programming</h2>
        <p class="mt-3 text-[var(--muted)]">A gentle start: input → process → output, print(), basics, comments & tasks</p>
        
      </div>
    </div>

    <!-- 1. Fundamental Working of Code -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">1) Fundamental Working of Code</h3>
        <p class="mt-2 text-[var(--muted)]">At its core, all programming follows a simple, fundamental process:</p>
        <p class="mt-2 text-[var(--muted)]">The job of the code is to take some form of input, perform a set of instructions on it, 
            and then provide a result as output.</p>
        <div class="mt-6 grid grid-cols-1 sm:grid-cols-3 gap-4">
          <div class="rounded-2xl border border-white/10 p-5 bg-white/5">
            <div class="text-3xl">⌨️</div>
            <h4 class="mt-2 font-semibold">Input</h4>
            <p class="text-sm text-[var(--muted)]">Data you give to the program</p>
          </div>
          <div class="rounded-2xl border border-white/10 p-5 bg-white/5">
            <div class="text-3xl">⚙️</div>
            <h4 class="mt-2 font-semibold">Process (Code)</h4>
            <p class="text-sm text-[var(--muted)]">Instructions the computer executes</p>
          </div>
          <div class="rounded-2xl border border-white/10 p-5 bg-white/5">
            <div class="text-3xl">📤</div>
            <h4 class="mt-2 font-semibold">Output</h4>
            <p class="text-sm text-[var(--muted)]">Result shown by the program</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Example: Search Engine -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">Example: Search Engine (Google)</h3>
        <ul class="mt-4 space-y-3 text-sm">
          <li><span class="num-badge text-[10px] text-slate-900 font-bold inline-flex items-center justify-center w-5 h-5 rounded-full mr-2">1</span><span class="font-semibold">Input:</span> Your search query (e.g., "what is Python?")</li>
          <li><span class="num-badge text-[10px] text-slate-900 font-bold inline-flex items-center justify-center w-5 h-5 rounded-full mr-2">2</span><span class="font-semibold">Process:</span> Google's program processes this input, searching its vast database.</li>
          <li><span class="num-badge text-[10px] text-slate-900 font-bold inline-flex items-center justify-center w-5 h-5 rounded-full mr-2">3</span><span class="font-semibold">Output:</span> The search results page.</li>
        </ul>
      </div>
    </div>
<!-- Database Note (Code Styled) -->
 
<section class="max-w-xl mx-auto my-6">
  <h2 class="text-2xl font-bold mb-3">📘 Note</h2>
  <pre class="bg-gray-900 text-green-400 text-lg p-4 rounded-xl shadow-md">
A database is a place where information is stored 
in an organized way so it can be found and used easily.
  </pre>
</section>
    <!-- 2. Our First Python Program: print() -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">2) Our First Python Program: <span class="code">print()</span></h3>
        <p class="mt-2 text-[var(--muted)]">The <span class="code">print()</span> function displays information on the screen.</p>
        <div class="mt-6 rounded-2xl border border-white/10 bg-black/40 p-5">
          <div class="text-xs text-[var(--muted)]">Python</div>
          <pre class="code text-sm">print("Hello")</pre>
        </div>
      </div>
    </div>


    <!-- Syntax Breakdown -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">Syntax Breakdown: <span class="code">print("Hello")</span></h3>
        <ul class="mt-4 space-y-3 text-sm">
          <li><span class="font-semibold">print</span>: Function name — a block of code that performs a specific task.</li>
          <li><span class="font-semibold">( )</span>: Parentheses — used to pass information to the function.</li>
          <li><span class="font-semibold">"Hello"</span>: Argument — the data we pass in (a <em>string</em> of text).</li>
        </ul>
      </div>
    </div>

    <!-- Output: Hello -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl text-center">
        <h3 class="text-2xl font-bold">The result of running <span class="code">print("Hello")</span> is:</h3>
        <div class="mt-6 rounded-2xl border border-white/10 bg-black/60 p-6 inline-block">
          <div class="code text-xl">Hello</div>
        </div>
        <p class="mt-4 text-sm"><span class="dot bg-sky-400"></span> <span class="ml-2 text-[var(--muted)]">Output shown on the screen</span></p>
      </div>
    </div>

    <!-- Key Definitions -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">Key Definitions</h3>
        <div class="mt-4 grid-auto">
          <div class="rounded-2xl border border-white/10 p-5 bg-white/5">
            <h4 class="font-semibold">Function</h4>
            <p class="text-sm text-[var(--muted)]">A command that completes a specific task.</p>
          </div>
          <div class="rounded-2xl border border-white/10 p-5 bg-white/5">
            <h4 class="font-semibold">Argument</h4>
            <p class="text-sm text-[var(--muted)]">Information provided to a function.</p>
          </div>
          <div class="rounded-2xl border border-white/10 p-5 bg-white/5">
            <h4 class="font-semibold">Output</h4>
            <p class="text-sm text-[var(--muted)]">What the program shows after it runs.</p>
          </div>
        </div>
      </div>
    </div>

    <!-- 3a. Printing Multiple Items -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">3a) Printing Multiple Items</h3>
        <p class="mt-2 text-[var(--muted)]">Separate items with commas. Python adds spaces automatically.</p>
        <div class="mt-6 rounded-2xl border border-white/10 bg-black/40 p-5">
          <div class="text-xs text-[var(--muted)]">Python</div>
<pre class="code text-sm">print("Hi", "My name is Ayesha", "I am a good girl")</pre>
        </div>
        <p class="mt-4 text-sm"><span class="dot bg-emerald-400"></span> Output: <span class="code">Hi My name is Ayesha I am a good girl</span></p>
      </div>
    </div>

    <!-- 3b. Printing on Different Lines -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">3b) Printing on Different Lines</h3>
        <div class="mt-6 rounded-2xl border border-white/10 bg-black/40 p-5">
          <div class="text-xs text-[var(--muted)]">Python</div>
<pre class="code text-sm">print("Hello, World!")
print("This is a new line.")</pre>
        </div>
        <p class="mt-4 text-sm"><span class="dot bg-emerald-400"></span> Output:
          <span class="block code">Hello, World!</span>
          <span class="block code">This is a new line.</span>
        </p>
      </div>
    </div>

    <!-- 3c. Numbers and Calculations -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">3c) Printing Numbers & Calculations</h3>
        <div class="mt-6 rounded-2xl border border-white/10 bg-black/40 p-5">
          <div class="text-xs text-[var(--muted)]">Python</div>
<pre class="code text-sm">print(20)       # prints a number
print(30 + 10)  # addition → 40
print(9 - 3)    # subtraction
print(6 * 7)    # multiplication
print(8 / 2)    # division</pre>
        </div>
      </div>
    </div>

    <!-- 4a. Character Set -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">4) Python Fundamentals — Character Set</h3>
        <ul class="mt-4 space-y-2 text-sm">
          <li>• Letters: A–Z, a–z</li>
          <li>• Digits: 0–9</li>
          <li>• Special symbols: +, -, *, /, #, etc.</li>
          <li>• Whitespace: space, tab, newline</li>
          <li class="text-[var(--muted)]">Python supports ASCII and Unicode.</li>
        </ul>
      </div>
    </div>

    <!-- 4b. Case Sensitivity -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">4) Python Fundamentals — Case Sensitivity</h3>
        <p class="mt-2 text-[var(--muted)]">Python treats uppercase and lowercase letters differently.</p>
        <div class="mt-6 grid sm:grid-cols-2 gap-4">
          <div class="rounded-2xl border border-white/10 p-5 bg-white/5">
            <p class="code">A ≠ a</p>
            <p class="code">Print ≠ print</p>
          </div>
          <div class="rounded-2xl border border-emerald-400/30 p-5 bg-emerald-400/10">
            <p class="text-sm">✅ Correct: use <span class="code">print</span> (all lowercase)</p>
          </div>
        </div>
      </div>
    </div>

    <!-- 5. Comments in Python -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">5) Comments in Python</h3>
        <p class="mt-2 text-[var(--muted)]">Comments explain your code and are ignored by Python.</p>
        <ul class="mt-4 space-y-2 text-sm">
          <li>• Single-line comments start with <span class="code">#</span></li>
          <li>• Multi-line comments are inside triple quotes <span class="code">""" ... """</span> or <span class="code">''' ... '''</span></li>
       <li>• Shortcut Key for Comments <span class="code"  style="color: rgb(238, 203, 7);"> &nbsp;&nbsp;&nbsp;  Press Ctrl + / </span></li>
        </ul>
      </div>
    </div>

    <!-- Single-line comment example -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-xl font-semibold">Single-line Comment</h3>
        <div class="mt-4 rounded-2xl border border-white/10 bg-black/40 p-5">
          <div class="text-xs text-[var(--muted)]">Python</div>
<pre class="code text-sm"> <span style="color: rgb(15, 129, 20);"> # This is a single-line comment.</span> 
print("Hello")  # This comment explains the print statement.</pre>
        </div>
      </div>
    </div>

    <!-- Multi-line comment example -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-xl font-semibold">Multi-line Comment</h3>
        <div class="mt-4 rounded-2xl border border-white/10 bg-black/40 p-5">
          <div class="text-xs text-[var(--muted)]">Python</div>
<pre class="code text-sm"><span style="color: rgb(15, 129, 20);">"""
This is a multi-line comment.
It can span across multiple lines and is useful
for writing detailed explanations.
"""</span>
print("This code will run.")</pre>
        </div>
      </div>
    </div>

    <!-- 6a. Interpreter -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">6)  Interpreter</h3>
        <p class="mt-2 text-[var(--muted)]">An interpreter executes code <em>line by line</em>. Python is an interpreted language.</p>
      </div>
    </div>

    <!-- 6b. File Extension -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">6) File Extension</h3>
        <p class="mt-2 text-[var(--muted)]">A short set of letters after the dot (<span class="code">.</span>) in a file name that tells the computer the file type and which program can open it.</p>
        <div class="mt-6 grid sm:grid-cols-2 gap-4 text-sm">
          <div class="rounded-2xl border border-white/10 bg-white/5 p-4">document.docx → <span class="code">.docx</span> (Word file)</div>
          <div class="rounded-2xl border border-white/10 bg-white/5 p-4">photo.jpg → <span class="code">.jpg</span> (image file)</div>
          <div class="rounded-2xl border border-white/10 bg-white/5 p-4">song.mp3 → <span class="code">.mp3</span> (audio file)</div>
          <div class="rounded-2xl border border-emerald-400/30 bg-emerald-400/10 p-4">program.py → <span class="code">.py</span> (Python file)</div>
        </div>
        <p class="mt-4 text-sm"><span class="dot bg-fuchsia-400"></span> In short: <span class="code">file extension = file type indicator</span>.</p>
      </div>
    </div>

    <!-- 6c. Reminder .py -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-2xl text-center">
        <h3 class="text-2xl font-bold">Python files are saved with the <span class="code">.py</span> extension</h3>
        <p class="mt-3 text-[var(--muted)]">Example: <span class="code">my_program.py</span></p>
      </div>
    </div>

    <!-- Task: Twinkle -->
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-2xl font-bold">Task</h3>
        <p class="mt-2">Write a program to print the <em>"Twinkle, Twinkle, Little Star"</em> poem.</p>
        <p class="mt-3 text-[var(--muted)]">Hint: use one <span class="code">print()</span> per line.</p>
     
     <!-- Cute Star Image -->
    <div class="mt-6 flex justify-center">
      <img src="star.jpg"
           alt="Twinkle Twinkle Little Star illustration"
           class="rounded-2xl shadow-soft max-h-64 hover:scale-105 transition-transform duration-300">
    </div>
      </div>
    </div>
    <!-- thank you slide -->
<div class="slide thankyou-slide">
  <canvas id="confetti-canvas"></canvas>
  <h1 class="thankyou-text">✨ Thank You ✨</h1>
  <p class="thankyou-sub">For Your Time & Attention</p>
  <!-- Right Side: Image -->
  <div class="thankyou-image">
    <img src="thankk.png" alt="Thank You">
  </div>
</div>

</div>

    <!-- (Optional) Sample Solution 
    <div class="slide px-8 py-10 w-full grid place-items-center">
      <div class="max-w-3xl">
        <h3 class="text-xl font-semibold">Sample Solution (for teachers)</h3>
        <div class="mt-3 rounded-2xl border border-white/10 bg-black/40 p-5">
          <div class="text-xs text-[var(--muted)]">Python</div>
<pre class="code text-sm">print("Twinkle, twinkle, little star,")
print("How I wonder what you are!")
print("Up above the world so high,")
print("Like a diamond in the sky.")</pre>
        </div>   -->
      </div>
      
    </div>
  </template>

  <script>
    // Build slides from template nodes to keep markup tidy
    const tpl = document.getElementById('tpl');
    const slidesHost = document.getElementById('slides');
    const nodes = Array.from(tpl.content.children);
    let slides = [];
    nodes.forEach(n => { n.classList.add('flex','items-center','justify-center'); n.style.minHeight = '68vh'; slides.push(n); });

    // State
    let index = 0;
    const total = slides.length;

    const pageNum = document.getElementById('pageNum');
    const pageTotal = document.getElementById('pageTotal');
    pageTotal.textContent = total;

    function render(){
      slidesHost.innerHTML = '';
      // simple page-turn feel by toggling classes
      const page = document.getElementById('page');
      page.classList.remove('enter','center','exit');
      page.classList.add('center');

      slidesHost.appendChild(slides[index]);
      pageNum.textContent = index + 1;
    }

    function next(){ if(index < total-1){ index++; renderFlip('next'); } }
    function prev(){ if(index > 0){ index--; renderFlip('prev'); } }

    function renderFlip(dir){
      const page = document.getElementById('page');
      page.classList.remove('enter','center','exit');
      page.classList.add(dir === 'next' ? 'exit' : 'enter');
      setTimeout(()=>{ render(); }, 140);
    }

    document.getElementById('next').addEventListener('click', next);
    document.getElementById('prev').addEventListener('click', prev);

    // Click regions for quick nav
    document.getElementById('page').addEventListener('click', (e)=>{
      const bounds = e.currentTarget.getBoundingClientRect();
      const mid = bounds.left + bounds.width/2;
      if(e.clientX > mid) next(); else prev();
    });

    // Keyboard support
    window.addEventListener('keydown', (e)=>{
      if(e.key === 'ArrowRight') next();
      if(e.key === 'ArrowLeft') prev();
    });

    // First render
    render();

   
  </script>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>

</body>
</html>

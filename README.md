<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Digital Clock</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #f0f0f0;
        }

        .container {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            margin: 20px;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        .tabs button {
            padding: 10px 20px;
            margin: 5px;
            border: none;
            background-color: #e0e0e0;
            cursor: pointer;
        }

        .tabs button.active {
            background-color: #007bff;
            color: white;
        }

        .clock {
            font-size: 48px;
            text-align: center;
            margin: 20px;
        }

        .timer-input, .alarm-input {
            display: flex;
            gap: 10px;
            margin: 20px 0;
        }

        input {
            padding: 5px;
            width: 60px;
        }

        button {
            padding: 8px 16px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="tabs">
            <button onclick="showTab('world-clock')" class="active">World Clock</button>
            <button onclick="showTab('timer')">Timer</button>
            <button onclick="showTab('stopwatch')">Stopwatch</button>
            <button onclick="showTab('alarm')">Alarm</button>
        </div>

        <div id="world-clock" class="tab-content active">
            <div class="clock" id="local-time"></div>
            <div class="clock" id="utc-time"></div>
        </div>

        <div id="timer" class="tab-content">
            <div class="timer-input">
                <input type="number" id="hours" placeholder="HH" min="0">
                <input type="number" id="minutes" placeholder="MM" min="0" max="59">
                <input type="number" id="seconds" placeholder="SS" min="0" max="59">
            </div>
            <button onclick="startTimer()">Start Timer</button>
            <div class="clock" id="timer-display">00:00:00</div>
        </div>

        <div id="stopwatch" class="tab-content">
            <div class="clock" id="stopwatch-display">00:00:00</div>
            <button onclick="startStopwatch()">Start</button>
            <button onclick="stopStopwatch()">Stop</button>
            <button onclick="resetStopwatch()">Reset</button>
        </div>

        <div id="alarm" class="tab-content">
            <div class="alarm-input">
                <input type="time" id="alarm-time">
                <button onclick="setAlarm()">Set Alarm</button>
            </div>
            <div id="alarm-status"></div>
        </div>
    </div>

    <script>
        // Tab navigation
        function showTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            document.querySelectorAll('.tabs button').forEach(btn => {
                btn.classList.remove('active');
            });
            document.getElementById(tabId).classList.add('active');
            event.currentTarget.classList.add('active');
        }

        // World Clock
        function updateClocks() {
            const now = new Date();
            document.getElementById('local-time').textContent = now.toLocaleTimeString();
            document.getElementById('utc-time').textContent = now.toUTCString().split(' ')[4];
        }
        setInterval(updateClocks, 1000);

        // Timer
        let timerInterval;
        function startTimer() {
            const hours = parseInt(document.getElementById('hours').value) || 0;
            const minutes = parseInt(document.getElementById('minutes').value) || 0;
            const seconds = parseInt(document.getElementById('seconds').value) || 0;
            
            let totalSeconds = hours * 3600 + minutes * 60 + seconds;
            
            timerInterval = setInterval(() => {
                totalSeconds--;
                if (totalSeconds < 0) {
                    clearInterval(timerInterval);
                    alert('Time is up!');
                    return;
                }
                
                const h = Math.floor(totalSeconds / 3600);
                const m = Math.floor((totalSeconds % 3600) / 60);
                const s = totalSeconds % 60;
                
                document.getElementById('timer-display').textContent = 
                    `${h.toString().padStart(2, '0')}:${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`;
            }, 1000);
        }

        // Stopwatch
        let stopwatchInterval;
        let stopwatchTime = 0;
        function startStopwatch() {
            stopwatchInterval = setInterval(() => {
                stopwatchTime++;
                const h = Math.floor(stopwatchTime / 3600);
                const m = Math.floor((stopwatchTime % 3600) / 60);
                const s = stopwatchTime % 60;
                document.getElementById('stopwatch-display').textContent = 
                    `${h.toString().padStart(2, '0')}:${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`;
            }, 1000);
        }

        function stopStopwatch() {
            clearInterval(stopwatchInterval);
        }

        function resetStopwatch() {
            stopwatchTime = 0;
            document.getElementById('stopwatch-display').textContent = '00:00:00';
        }

        // Alarm
        let alarmInterval;
        function setAlarm() {
            const alarmTime = document.getElementById('alarm-time').value;
            const [alarmHours, alarmMinutes] = alarmTime.split(':');
            
            alarmInterval = setInterval(() => {
                const now = new Date();
                if (now.getHours() == alarmHours && now.getMinutes() == alarmMinutes) {
                    alert('Alarm!');
                    clearInterval(alarmInterval);
                    document.getElementById('alarm-status').textContent = 'Alarm triggered!';
                }
            }, 1000);
            
            document.getElementById('alarm-status').textContent = `Alarm<header>

<!--
  <<< Author notes: Course header >>>
  Include a 1280×640 image, course title in sentence case, and a concise description in emphasis.
  In your repository settings: enable template repository, add your 1280×640 social image, auto delete head branches.
  Add your open source license, GitHub uses MIT license.
-->

# GitHub Pages

_Create a site or blog from your GitHub repositories with GitHub Pages._

</header>

<!--
  <<< Author notes: Course start >>>
  Include start button, a note about Actions minutes,
  and tell the learner why they should take the course.
-->

## Welcome

With GitHub Pages, you can host project blogs, documentation, resumes, portfolios, or any other static content you'd like. Your GitHub repository can easily become its own website. In this course, we'll show you how to set up your own site or blog using GitHub Pages.

- **Who is this for**: Beginners, students, project maintainers, small businesses.
- **What you'll learn**: How to build a GitHub Pages site.
- **What you'll build**: We'll build a simple GitHub Pages site with a blog. We'll use [Jekyll](https://jekyllrb.com), a static site generator.
- **Prerequisites**: If you need to learn about branches, commits, and pull requests, take [Introduction to GitHub](https://github.com/skills/introduction-to-github) first.
- **How long**: This course takes less than one hour to complete.

In this course, you will:

1. Enable GitHub Pages
2. Configure your site
3. Customize your home page
4. Create a blog post
5. Merge your pull request

### How to start this course

<!-- For start course, run in JavaScript:
'https://github.com/new?' + new URLSearchParams({
  template_owner: 'skills',
  template_name: 'github-pages',
  owner: '@me',
  name: 'skills-github-pages',
  description: 'My clone repository',
  visibility: 'public',
}).toString()
-->

[![start-course](https://user-images.githubusercontent.com/1221423/235727646-4a590299-ffe5-480d-8cd5-8194ea184546.svg)](https://github.com/new?template_owner=skills&template_name=github-pages&owner=%40me&name=skills-github-pages&description=My+clone+repository&visibility=public)

1. Right-click **Start course** and open the link in a new tab.
2. In the new tab, most of the prompts will automatically fill in for you.
   - For owner, choose your personal account or an organization to host the repository.
   - We recommend creating a public repository, as private repositories will [use Actions minutes](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions).
   - Scroll down and click the **Create repository** button at the bottom of the form.
3. After your new repository is created, wait about 20 seconds, then refresh the page. Follow the step-by-step instructions in the new repository's README.

<footer>

<!--
  <<< Author notes: Footer >>>
  Add a link to get support, GitHub status page, code of conduct, license link.
-->

---

Get help: [Post in our discussion board](https://github.com/orgs/skills/discussions/categories/github-pages) &bull; [Review the GitHub status page](https://www.githubstatus.com/)

&copy; 2023 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)

</footer>

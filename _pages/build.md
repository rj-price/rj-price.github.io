---
layout: none
title: build
nav: false
permalink: /build/
---

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minecraft Survival Project Tracker</title>
    <style>
        :root {
            --mc-green: #38873c;
            --mc-dark-green: #2a632d;
            --mc-stone: #4a4a4a;
            --mc-light: #f4f4f4;
            --mc-dark: #1a1a1a;
            --mc-gold: #ffcc00;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--mc-dark);
            color: var(--mc-light);
            margin: 0;
            padding: 20px;
            line-height: 1.6;
        }

        header {
            text-align: center;
            padding: 30px 20px;
            background: linear-gradient(rgba(0,0,0,0.8), rgba(0,0,0,0.8)), url('https://images.unsplash.com/photo-1627398242454-45a1465c2479?auto=format&fit=crop&q=80&w=1000');
            background-size: cover;
            border-radius: 15px;
            margin-bottom: 20px;
            border: 3px solid var(--mc-stone);
        }

        .progress-container {
            max-width: 1200px;
            margin: 0 auto 30px auto;
            background: #333;
            border-radius: 10px;
            padding: 15px;
            text-align: center;
        }

        .progress-bar-bg {
            background: #111;
            height: 20px;
            border-radius: 10px;
            overflow: hidden;
            margin-top: 10px;
        }

        #progress-fill {
            background: linear-gradient(90deg, var(--mc-dark-green), var(--mc-green));
            width: 0%;
            height: 100%;
            transition: width 0.5s ease;
        }

        .filter-container {
            display: flex;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 30px;
        }

        .filter-btn {
            background-color: var(--mc-stone);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
            text-transform: uppercase;
        }

        .filter-btn.active { background-color: var(--mc-green); }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 15px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .card {
            background-color: #2a2a2a;
            border-radius: 8px;
            padding: 20px;
            border-bottom: 4px solid var(--mc-stone);
            display: flex;
            flex-direction: column;
        }

        .card.completed {
            opacity: 0.6;
            border-bottom-color: var(--mc-gold);
        }

        .card.completed h3 { text-decoration: line-through; color: #888; }

        .card h3 { margin: 10px 0; font-size: 1.1rem; color: var(--mc-green); }

        .category-tag {
            font-size: 0.65rem;
            background: #444;
            padding: 2px 6px;
            border-radius: 3px;
            text-transform: uppercase;
            align-self: flex-start;
        }

        .checkbox-container {
            margin-top: auto;
            display: flex;
            align-items: center;
            gap: 10px;
            cursor: pointer;
            padding-top: 10px;
            border-top: 1px solid #3d3d3d;
        }

        .hidden { display: none; }
    </style>
</head>
<body>

<header>
    <h1>Minecraft Survival Project Tracker</h1>
</header>

<div class="progress-container">
    <span id="progress-text">0/0 Projects Completed (0%)</span>
    <div class="progress-bar-bg">
        <div id="progress-fill"></div>
    </div>
</div>

<div class="filter-container">
    <button class="filter-btn active" onclick="filterSelection('all')">All</button>
    <button class="filter-btn" onclick="filterSelection('combat')">Exploration & Combat</button>
    <button class="filter-btn" onclick="filterSelection('farm')">Farms & Tech</button>
    <button class="filter-btn" onclick="filterSelection('build')">Building & Aesthetic</button>
</div>

<div class="grid" id="projectGrid">
    </div>

<script>
    const projects = [
        // CATEGORY: COMBAT/EXPLORATION
        { id: 'c1', cat: 'combat', title: 'Raid Bastion', desc: '', done: true },
        { id: 'c2', cat: 'combat', title: 'Full Netherite Gear', desc: '', done: true },
        { id: 'c3', cat: 'combat', title: 'Find Stronghold', desc: '', done: true },
        { id: 'c4', cat: 'combat', title: 'Defeat the Dragon', desc: '', done: true },
        { id: 'c5', cat: 'combat', title: 'Get Wings & Shulker Boxes', desc: '', done: true },
        { id: 'c6', cat: 'combat', title: 'Raid Ocean Monument', desc: '', done: false },
        { id: 'c7', cat: 'combat', title: 'Raid Pillager Outpost', desc: '', done: true },
        { id: 'c8', cat: 'combat', title: 'Raid Ancient City', desc: '', done: false },
        { id: 'c9', cat: 'combat', title: 'Explore Desert Temple', desc: '', done: true },
        { id: 'c10', cat: 'combat', title: 'Summon & Fight Wither', desc: '', done: true },
        { id: 'c11', cat: 'combat', title: 'Build a Beacon', desc: '', done: true },
        { id: 'c12', cat: 'combat', title: 'Cheese Withers for Beacons', desc: '', done: false },
        { id: 'c13', cat: 'combat', title: 'Get All Frogs', desc: '', done: true },
        { id: 'c14', cat: 'combat', title: 'Get Sniffers', desc: '', done: true },
        { id: 'c15', cat: 'combat', title: 'Excavate Trail Ruins', desc: '', done: false },

        // CATEGORY: FARMS
        { id: 'f1', cat: 'farm', title: 'Super Smelter', desc: '', done: false },
        { id: 'f2', cat: 'farm', title: 'Auto Craft/Compost Iron Farm', desc: '', done: true },
        { id: 'f3', cat: 'farm', title: 'Creeper Farm', desc: '', done: true },
        { id: 'f4', cat: 'farm', title: 'Flower & Dye Farm', desc: '', done: false },
        { id: 'f5', cat: 'farm', title: 'Triple Spider XP Farm', desc: '', done: true },
        { id: 'f6', cat: 'farm', title: 'Pumpkin/Melon (Auto Craft)', desc: '', done: false },
        { id: 'f7', cat: 'farm', title: 'Sugar Cane Farm', desc: '', done: true },
        { id: 'f8', cat: 'farm', title: 'Paper Auto Crafter', desc: '', done: true },
        { id: 'f9', cat: 'farm', title: 'Cactus Smelter Repairer', desc: '', done: true },
        { id: 'f10', cat: 'farm', title: 'Kelp Bonemeal/Fuel Farm', desc: '', done: false },
        { id: 'f11', cat: 'farm', title: 'Wool Farm', desc: '', done: true },
        { id: 'f12', cat: 'farm', title: 'Bamboo Farm', desc: '', done: true },
        { id: 'f13', cat: 'farm', title: 'Automatic Carrot Farm', desc: '', done: false },
        { id: 'f14', cat: 'farm', title: 'Gold Farm', desc: '', done: true },
        { id: 'f15', cat: 'farm', title: 'Automate Gold Output', desc: '', done: false },
        { id: 'f16', cat: 'farm', title: 'Slime Farm', desc: '', done: true },
        { id: 'f17', cat: 'farm', title: 'Breeze Farm', desc: '', done: false },
        { id: 'f18', cat: 'farm', title: 'Guardian Farm', desc: 'Drain an entire Ocean Monument for prismarine and XP.', done: false },
        { id: 'f19', cat: 'farm', title: 'Bastion Frog Light Farm', desc: '', done: false },
        { id: 'f20', cat: 'farm', title: 'Sniffer Sanctuary (Cart Collection)', desc: '', done: false },
        { id: 'f21', cat: 'farm', title: 'Automatic Bee Farm', desc: 'Create an automated bee farm for honeycomb and honey blocks.', done: false },

        // CATEGORY: BUILDING
        { id: 'b1', cat: 'build', title: 'Barn & Silo', desc: '', done: true },
        { id: 'b2', cat: 'build', title: 'Build Bridge', desc: '', done: false },
        { id: 'b3', cat: 'build', title: 'Decorate Industrial Zone', desc: '', done: false },
        { id: 'b4', cat: 'build', title: 'Water Features (Lake/Waterfall)', desc: '', done: false },
        { id: 'b5', cat: 'build', title: 'Lighthouse', desc: '', done: false },
        { id: 'b6', cat: 'build', title: 'Custom Trees', desc: '', done: false },
        { id: 'b7', cat: 'build', title: 'Villager District', desc: '', done: false }
        { id: 'b8', cat: 'build', title: 'Floating Archipelago', desc: 'Build a series of floating islands connected by massive chains.', done: false },
        { id: 'b9', cat: 'build', title: 'Nether Hub', desc: 'A grand central station in the Nether with blue ice pathways.', done: false },
        { id: 'b10', cat: 'build', title: 'Cyberpunk Crater City', desc: 'Dig a massive perimeter and build a neon-lit, vertical city inside the hole.', done: false },
        { id: 'b11', cat: 'build', title: 'Global Map Room', desc: 'A 50x50 map wall showing your entire explored world, housed in a glass-domed observatory.', done: false },
        { id: 'b12', cat: 'build', title: 'Abandoned overgrown Ruins', desc: 'Construct a sprawling ancient ruin overtaken by nature.', done: false },
        { id: 'b13', cat: 'build', title: 'Custom Biome Transform', desc: 'Take a desert or plains area and manually turn every block into a custom 'Alien' or 'Autumnal' forest.', done: false },

    ];

    function init() {
        const grid = document.getElementById('projectGrid');
        const saved = JSON.parse(localStorage.getItem('mcWorldProgress')) || {};

        projects.forEach(p => {
            const isDone = saved.hasOwnProperty(p.id) ? saved[p.id] : p.done;
            const card = document.createElement('div');
            card.className = `card ${p.cat} ${isDone ? 'completed' : ''}`;
            card.id = p.id;
            card.innerHTML = `
                <span class="category-tag">${p.cat}</span>
                <h3>${p.title}</h3>
                <p>${p.desc}</p>
                <label class="checkbox-container">
                    <input type="checkbox" ${isDone ? 'checked' : ''} onchange="toggle('${p.id}')"> Finished
                </label>
            `;
            grid.appendChild(card);
        });
        updateProgress();
    }

    function toggle(id) {
        const card = document.getElementById(id);
        const checked = card.querySelector('input').checked;
        card.classList.toggle('completed', checked);
        
        // Save to local storage
        const saved = JSON.parse(localStorage.getItem('mcWorldProgress')) || {};
        saved[id] = checked;
        localStorage.setItem('mcWorldProgress', JSON.stringify(saved));
        
        updateProgress();
    }

    function updateProgress() {
        const total = projects.length;
        const done = document.querySelectorAll('input:checked').length;
        const percent = Math.round((done / total) * 100);
        
        document.getElementById('progress-text').textContent = `${done}/${total} Projects Completed (${percent}%)`;
        document.getElementById('progress-fill').style.width = percent + '%';
    }

    function filterSelection(c) {
        let x = document.getElementsByClassName("card");
        if (c == "all") c = "";
        for (let i = 0; i < x.length; i++) {
            x[i].classList.add("hidden");
            if (x[i].className.indexOf(c) > -1) x[i].classList.remove("hidden");
        }
        
        let btns = document.getElementsByClassName("filter-btn");
        for (let i = 0; i < btns.length; i++) btns[i].classList.remove("active");
        event.currentTarget.classList.add("active");
    }

    window.onload = init;
</script>

</body>

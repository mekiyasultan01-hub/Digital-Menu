<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>The Urban Roast - Digital Menu</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
  <style>
    body { font-family: 'Inter', sans-serif; }
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  </style>
</head>
<body class="bg-amber-50/30 text-stone-800 min-h-screen pb-16">

  <!-- Header Header -->
  <header class="bg-amber-900 text-amber-50 pt-8 pb-6 px-4 text-center shadow-md">
    <span class="text-xs uppercase tracking-widest text-amber-300 font-semibold">Welcome To</span>
    <h1 class="text-3xl font-bold tracking-tight mt-1">The Urban Roast</h1>
    <p class="text-xs text-amber-200/80 mt-1">Artisanal Coffee & Fresh Kitchen</p>
    <div class="mt-3 inline-flex items-center gap-2 bg-amber-950/40 px-3 py-1 rounded-full text-xs text-amber-200">
      <span>📍 Table Menu</span> • <span>📶 Wi-Fi: UrbanRoast2026</span>
    </div>
  </header>

  <!-- Sticky Controls Bar -->
  <div class="sticky top-0 z-10 bg-white/95 backdrop-blur-md shadow-sm border-b border-amber-100">
    <!-- Search Bar -->
    <div class="p-3 pb-2 max-w-md mx-auto">
      <div class="relative">
        <input 
          type="text" 
          id="searchInput" 
          placeholder="Search items or ingredients..." 
          class="w-full bg-stone-100 text-sm pl-9 pr-4 py-2 rounded-xl focus:outline-none focus:ring-2 focus:ring-amber-700/50"
          oninput="filterMenu()"
        />
        <svg class="w-4 h-4 text-stone-400 absolute left-3 top-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
      </div>
    </div>

    <!-- Category Tabs -->
    <div class="flex overflow-x-auto no-scrollbar gap-2 px-3 pb-3 max-w-md mx-auto" id="categoryTabs">
      <!-- Injected by JS -->
    </div>
  </div>

  <!-- Main Menu Display -->
  <main class="max-w-md mx-auto px-4 pt-4" id="menuContainer">
    <!-- Injected by JS -->
  </main>

  <!-- Footer -->
  <footer class="max-w-md mx-auto text-center mt-8 px-4 text-stone-400 text-xs">
    <p>Please inform your server of any severe allergies.</p>
    <p class="mt-1">© 2026 The Urban Roast Cafe. All rights reserved.</p>
  </footer>

  <script>
    // EDIT YOUR MENU ITEMS HERE
    const menuData = [
      {
        category: "Coffee & Espresso",
        items: [
          { name: "Double Espresso", price: "$3.50", desc: "Rich single-origin Arabica with a smooth crema.", tags: ["Popular"] },
          { name: "Oat Milk Flat White", price: "$4.75", desc: "Velvety steamed oat milk over double ristretto.", tags: ["Vegan"] },
          { name: "Vanilla Bean Latte", price: "$5.25", desc: "House-made Madagascar vanilla syrup, espresso, and steam milk.", tags: [] },
          { name: "Cold Brew Reserve", price: "$4.50", desc: "Steeped for 18 hours. Smooth, low acidity, naturally sweet.", tags: ["Popular"] }
        ]
      },
      {
        category: "Bakery & Treats",
        items: [
          { name: "Butter Croissant", price: "$3.75", desc: "Flaky, golden layered French pastry baked fresh daily.", tags: [] },
          { name: "Almond Twice-Baked", price: "$4.50", desc: "Filled with rich almond frangipane and toasted flakes.", tags: ["Popular"] },
          { name: "Vegan Blueberry Scone", price: "$4.00", desc: "Made with coconut oil and fresh organic blueberries.", tags: ["Vegan"] }
        ]
      },
      {
        category: "All-Day Kitchen",
        items: [
          { name: "Avocado Sourdough Toast", price: "$11.50", desc: "Smashed hass avocado, radishes, chili flakes, sea salt on artisanal sourdough.", tags: ["Vegan"] },
          { name: "Truffle Mushroom Toast", price: "$13.00", desc: "Sautéed wild mushrooms, thyme, white truffle oil, and poached egg.", tags: ["Popular"] },
          { name: "Acai Superfood Bowl", price: "$12.00", desc: "Organic acai blend topped with chia seeds, banana, berry granola, and peanut butter.", tags: ["Vegan", "Gluten-Free"] }
        ]
      }
    ];

    let currentCategory = "All";

    function renderCategories() {
      const tabs = document.getElementById("categoryTabs");
      const categories = ["All", ...menuData.map(c => c.category)];
      
      tabs.innerHTML = categories.map(cat => `
        <button 
          onclick="selectCategory('${cat}')"
          class="px-4 py-1.5 rounded-full text-xs font-medium whitespace-nowrap transition-all ${
            currentCategory === cat 
              ? 'bg-amber-900 text-white shadow-sm' 
              : 'bg-stone-100 text-stone-600 hover:bg-stone-200'
          }"
        >
          ${cat}
        </button>
      `).join("");
    }

    function selectCategory(cat) {
      currentCategory = cat;
      renderCategories();
      filterMenu();
    }

    function filterMenu() {
      const query = document.getElementById("searchInput").value.toLowerCase();
      const container = document.getElementById("menuContainer");
      
      let html = "";

      menuData.forEach(catGroup => {
        if (currentCategory !== "All" && currentCategory !== catGroup.category) return;

        const filteredItems = catGroup.items.filter(item => 
          item.name.toLowerCase().includes(query) || 
          item.desc.toLowerCase().includes(query)
        );

        if (filteredItems.length > 0) {
          html += `
            <section class="mb-6">
              <h2 class="text-sm font-bold uppercase tracking-wider text-amber-950/70 border-b border-amber-200/60 pb-1.5 mb-3">
                ${catGroup.category}
              </h2>
              <div class="space-y-3">
                ${filteredItems.map(item => `
                  <div class="bg-white p-3.5 rounded-xl shadow-sm border border-stone-100 flex justify-between gap-3">
                    <div class="flex-1">
                      <div class="flex items-center gap-2">
                        <h3 class="text-sm font-semibold text-stone-900">${item.name}</h3>
                        ${item.tags.map(t => `<span class="text-[10px] px-1.5 py-0.5 rounded font-medium ${t === 'Vegan' ? 'bg-emerald-100 text-emerald-800' : 'bg-amber-100 text-amber-800'}">${t}</span>`).join('')}
                      </div>
                      <p class="text-xs text-stone-500 mt-1 leading-relaxed">${item.desc}</p>
                    </div>
                    <span class="text-sm font-bold text-amber-900 self-start">${item.price}</span>
                  </div>
                `).join('')}
              </div>
            </section>
          `;
        }
      });

      container.innerHTML = html || `<p class="text-center text-sm text-stone-400 py-8">No menu items found matching "${query}".</p>`;
    }

    // Initial Load
    renderCategories();
    filterMenu();
  </script>
</body>
</html>

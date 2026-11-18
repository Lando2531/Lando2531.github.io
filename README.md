# Lando2531.github.io
Christmas list

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Christmas Wishlist</title>

  <style>
    :root {
      --red: #9e252d;
      --green: #174b35;
      --gold: #d6ad55;
      --cream: #fffaf0;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: Georgia, serif;
      color: #26372d;
      background:
        radial-gradient(circle at 10% 15%, white 0 2px, transparent 3px),
        radial-gradient(circle at 80% 35%, white 0 2px, transparent 3px),
        linear-gradient(135deg, #f8f1e3, #e6efe7);
      background-size: 80px 80px, 110px 110px, auto;
      min-height: 100vh;
    }

    header {
      text-align: center;
      padding: 45px 18px 35px;
      color: white;
      background: linear-gradient(120deg, var(--green), #286448);
      border-bottom: 5px solid var(--gold);
    }

    header h1 {
      font-size: clamp(2rem, 7vw, 3.5rem);
      margin: 10px 0;
    }

    header p {
      margin: 0;
      font-size: 1.05rem;
    }

    .snowflake {
      font-size: 2rem;
    }

    main {
      width: min(900px, 92%);
      margin: 30px auto 60px;
    }

    .panel {
      background: var(--cream);
      border: 1px solid #e0d4b8;
      border-radius: 14px;
      padding: 22px;
      margin-bottom: 22px;
      box-shadow: 0 5px 18px #183b2b12;
    }

    h2 {
      color: var(--red);
      margin-top: 0;
    }

    input, button, textarea, select {
      font: inherit;
    }

    input, select, textarea {
      width: 100%;
      padding: 12px;
      border: 1px solid #c8d2c8;
      border-radius: 8px;
      background: white;
    }

    textarea {
      margin-top: 12px;
      min-height: 75px;
      resize: vertical;
    }

    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    .wide {
      grid-column: 1 / -1;
    }

    button {
      border: 0;
      border-radius: 9px;
      padding: 12px 17px;
      cursor: pointer;
      background: var(--green);
      color: white;
    }

    button:hover {
      opacity: .9;
    }

    .actions {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 14px;
    }

    .gift {
      border: 1px solid #e1d6c0;
      border-left: 5px solid var(--green);
      border-radius: 10px;
      padding: 16px;
      margin: 12px 0;
      background: white;
    }

    .gift h3 {
      margin: 0 0 8px;
      color: var(--green);
    }

    .gift p {
      margin: 6px 0;
      overflow-wrap: anywhere;
    }

    .gift a {
      color: var(--green);
    }

    .gift button {
      margin: 10px 6px 0 0;
      padding: 8px 12px;
      font-size: .9rem;
    }

    .delete {
      background: var(--red);
    }

    .top-pick {
      color: #9b741e;
      font-weight: bold;
    }

    .empty {
      text-align: center;
      padding: 25px 10px;
      color: #69766c;
    }

    .footer {
      text-align: center;
      padding: 20px;
      color: var(--green);
    }

    @media (max-width: 600px) {
      .form-grid {
        grid-template-columns: 1fr;
      }

      .wide {
        grid-column: auto;
      }

      .panel {
        padding: 17px;
      }
    }
  </style>
</head>

<body>

  <header>
    <div class="snowflake">❄️ 🎁 ❄️</div>
    <h1>My Christmas Wishlist</h1>
    <p>A little list of things I'd love this Christmas</p>
    <div class="snowflake">🎄 ✨ 🎄</div>
  </header>

  <main>

    <section class="panel">
      <h2>🎁 Add a Gift</h2>

      <form id="giftForm">
        <div class="form-grid">
          <input
            id="giftName"
            placeholder="Gift name"
            required
          >

          <input
            id="giftPrice"
            placeholder="Price (optional)"
          >

          <input
            id="giftLink"
            type="url"
            placeholder="Product link (optional)"
          >

          <select id="giftCategory">
            <option>Electronics</option>
            <option>Clothing</option>
            <option>Hobbies</option>
            <option>Halloween</option>
            <option>Gaming</option>
            <option>Other</option>
          </select>

          <label class="wide">
            <input
              id="giftTopPick"
              type="checkbox"
              style="width:auto"
            >
            ⭐ Mark as a top pick
          </label>
        </div>

        <textarea
          id="giftNotes"
          placeholder="Notes, size, color, or other details..."
        ></textarea>

        <div class="actions">
          <button type="submit">Add to Wishlist</button>
        </div>
      </form>
    </section>

    <section class="panel">
      <h2>🎄 My Wishlist</h2>

      <div class="form-grid">
        <input
          id="searchInput"
          placeholder="Search gifts..."
          aria-label="Search gifts"
        >

        <select id="sortSelect" aria-label="Sort gifts">
          <option value="added">Recently added</option>
          <option value="name">Name A–Z</option>
          <option value="category">Category</option>
          <option value="top">Top picks first</option>
        </select>
      </div>

      <div id="giftList"></div>

      <div class="actions">
        <button type="button" onclick="shareWishlist()">
          Share Wishlist
        </button>

        <button type="button" onclick="exportBackup()">
          Download Backup
        </button>
      </div>
    </section>

    <div class="footer">
      <p>❄️ Merry Christmas! ❄️</p>
    </div>

  </main>

  <script>
    const STORAGE_KEY = "christmasWishlist2026";

    function getGifts() {
      try {
        return JSON.parse(
          localStorage.getItem(STORAGE_KEY) || "[]"
        );
      } catch {
        return [];
      }
    }

    function saveGifts(gifts) {
      localStorage.setItem(
        STORAGE_KEY,
        JSON.stringify(gifts)
      );
    }

    const giftForm = document.getElementById("giftForm");

    giftForm.addEventListener("submit", function(event) {
      event.preventDefault();

      const gifts = getGifts();

      gifts.push({
        id: Date.now(),
        name: document.getElementById("giftName").value.trim(),
        price: document.getElementById("giftPrice").value.trim(),
        link: document.getElementById("giftLink").value.trim(),
        category: document.getElementById("giftCategory").value,
        notes: document.getElementById("giftNotes").value.trim(),
        topPick: document.getElementById("giftTopPick").checked
      });

      saveGifts(gifts);
      giftForm.reset();
      renderGifts();
    });

    function renderGifts() {
      const list = document.getElementById("giftList");
      const search = document.getElementById("searchInput")
        .value.toLowerCase();

      const sort = document.getElementById("sortSelect").value;

      let gifts = getGifts().filter(gift =>
        gift.name.toLowerCase().includes(search) ||
        gift.category.toLowerCase().includes(search)
      );

      if (sort === "name") {
        gifts.sort((a, b) => a.name.localeCompare(b.name));
      } else if (sort === "category") {
        gifts.sort((a, b) =>
          a.category.localeCompare(b.category)
        );
      } else if (sort === "top") {
        gifts.sort((a, b) =>
          Number(b.topPick) - Number(a.topPick)
        );
      } else {
        gifts.reverse();
      }

      list.innerHTML = "";

      if (gifts.length === 0) {
        list.innerHTML = `
          <div class="empty">
            <p>🎁 Your wishlist is waiting for some Christmas magic!</p>
          </div>
        `;
        return;
      }

      gifts.forEach(gift => {
        const card = document.createElement("article");
        card.className = "gift";

        const title = document.createElement("h3");
        title.textContent = gift.name;
        card.appendChild(title);

        if (gift.topPick) {
          const pick = document.createElement("p");
          pick.className = "top-pick";
          pick.textContent = "⭐ Top Pick";
          card.appendChild(pick);
        }

        [
          gift.category,
          gift.price ? "Price: " + gift.price : "",
          gift.notes
        ].filter(Boolean).forEach(detail => {
          const p = document.createElement("p");
          p.textContent = detail;
          card.appendChild(p);
        });

        if (gift.link) {
          const link = document.createElement("a");
          link.href = gift.link;
          link.target = "_blank";
          link.rel = "noopener noreferrer";
          link.textContent = "View Gift";
          card.appendChild(link);
        }

        const remove = document.createElement("button");
        remove.className = "delete";
        remove.textContent = "Remove";

        remove.addEventListener("click", () => {
          if (confirm("Remove this gift from your wishlist?")) {
            saveGifts(
              getGifts().filter(item => item.id !== gift.id)
            );
            renderGifts();
          }
        });

        card.appendChild(document.createElement("br"));
        card.appendChild(remove);

        list.appendChild(card);
      });
    }

    async function shareWishlist() {
      const gifts = getGifts();

      if (!gifts.length) {
        alert("Add some gifts before sharing your wishlist!");
        return;
      }

      const text = "🎄 My Christmas Wishlist 🎄\n\n" +
        gifts.map(gift =>
          (gift.topPick ? "⭐ TOP PICK: " : "") +
          gift.name +
          (gift.price ? " — " + gift.price : "") +
          "\nCategory: " + gift.category +
          (gift.link ? "\n" + gift.link : "") +
          (gift.notes ? "\n" + gift.notes : "")
        ).join("\n\n");

      if (navigator.share) {
        try {
          await navigator.share({
            title: "My Christmas Wishlist",
            text: text
          });
        } catch (error) {
          // Sharing was cancelled.
        }
      } else {
        try {
          await navigator.clipboard.writeText(text);
          alert("Wishlist copied! Paste it into a message.");
        } catch {
          alert("Sharing isn't available in this browser.");
        }
      }
    }

    function exportBackup() {
      const data = JSON.stringify(getGifts(), null, 2);
      const blob = new Blob([data], {
        type: "application/json"
      });

      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");

      a.href = url;
      a.download = "christmas-wishlist-backup.json";
      a.click();

      URL.revokeObjectURL(url);
    }

    document.getElementById("searchInput")
      .addEventListener("input", renderGifts);

    document.getElementById("sortSelect")
      .addEventListener("change", renderGifts);

    renderGifts();
  </script>

</body>
</html>

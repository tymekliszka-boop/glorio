<!doctype html>
<html lang="pl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Nowoczesny Blog</title>
  <style>
    :root{
      --bg:#ffffff;
      --text:#1c1c1c;
      --accent:#8fd19a; /* jasno zielony */
      --muted:#6b6b6b;
      --card:#f8f9fa;
      --radius:12px;
      --max-width:1100px;
      font-family: Inter, system-ui, Arial, sans-serif;
    }

    *{box-sizing:border-box}
    body{margin:0;background:var(--bg);color:var(--text);line-height:1.45}
    .container{max-width:var(--max-width);margin:0 auto;padding:24px}

    header{display:flex;align-items:center;justify-content:space-between;padding:18px 0}
    .brand{display:flex;align-items:center;gap:12px}
    .logo{width:56px;height:56px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#7dc98a);display:flex;align-items:center;justify-content:center;color:white;font-weight:700;font-size:20px}
    nav{display:flex;gap:16px;align-items:center}
    nav a{color:var(--text);text-decoration:none;padding:8px 10px;border-radius:8px}
    nav a.cta{background:var(--accent);color:#fff}

    .hero{display:flex;flex-direction:column;gap:12px;padding:28px 0}
    .hero h1{margin:0;font-size:28px}
    .hero p{margin:0;color:var(--muted)}

    .grid{display:grid;grid-template-columns:1fr 320px;gap:24px;margin-top:18px}
    main{min-width:0}
    aside .card{background:var(--card);padding:16px;border-radius:var(--radius);margin-bottom:16px}

    .posts{display:grid;grid-template-columns:repeat(2,1fr);gap:16px}
    .post{background:var(--card);padding:14px;border-radius:12px;overflow:hidden;display:flex;flex-direction:column;gap:10px}
    .post img{width:100%;height:160px;object-fit:cover;border-radius:8px}
    .post h3{margin:0;font-size:18px}
    .meta{font-size:13px;color:var(--muted)}

    .gallery{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
    .gallery img{width:100%;height:110px;object-fit:cover;border-radius:8px;cursor:pointer;transition:transform .15s}
    .gallery img:hover{transform:scale(1.03)}

    form input, form textarea, .search{width:100%;padding:10px;border-radius:8px;border:1px solid #e6e6e6}
    button{background:var(--accent);border:0;color:white;padding:10px 14px;border-radius:8px;cursor:pointer}
    .filters{display:flex;gap:8px;flex-wrap:wrap}

    /* modal */
    .modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,0.55);z-index:50;padding:20px}
    .modal.open{display:flex}
    .modal .inner{max-width:1000px;background:white;padding:12px;border-radius:12px;overflow:auto}
    .modal img{width:100%;height:auto;border-radius:8px}

    /* single post view */
    .post-view img{width:100%;height:260px;object-fit:cover;border-radius:8px}
    .tags{display:flex;gap:8px;flex-wrap:wrap}

    /* responsive */
    @media (max-width:900px){
      .grid{grid-template-columns:1fr}
      .posts{grid-template-columns:1fr}
      .gallery{grid-template-columns:repeat(2,1fr)}
      .logo{width:48px;height:48px;font-size:18px}
    }
    @media (max-width:480px){
      .gallery img{height:90px}
      header{padding:12px 0}
      .hero h1{font-size:20px}
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="brand">
        <div class="logo" id="logo-placeholder">NB</div>
        <div>
          <div style="font-weight:700">Nowoczesny Blog</div>
          <div style="font-size:13px;color:var(--muted)">krótkie opisy, inspiracje, zdjęcia</div>
        </div>
      </div>
      <nav>
        <a href="#" data-nav="home">Home</a>
        <a href="#" data-nav="gallery">Galeria</a>
        <a href="#" data-nav="contact">Kontakt</a>
        <a class="cta" href="#" data-nav="newpost">Nowy wpis</a>
      </nav>
    </header>

    <section class="hero">
      <h1>Blog o designie i fotografii</h1>
      <p>Świeże wpisy, krótka galeria najlepszych zdjęć i prosty sposób dodawania treści.</p>
    </section>

    <div class="grid">
      <main>
        <div id="home-view">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <div class="filters">
              <select id="category-filter" style="padding:10px;border-radius:8px;border:1px solid #e6e6e6">
                <option value="">Wszystkie kategorie</option>
              </select>
              <input class="search" id="search" placeholder="Szukaj wpisów..." />
            </div>
            <div style="font-size:13px;color:var(--muted)">Liczba wpisów: <span id="posts-count">0</span></div>
          </div>

          <div class="posts" id="posts-list" style="margin-top:14px"></div>
        </div>

        <div id="post-view" style="display:none" class="post-view"></div>

        <div id="gallery-view" style="display:none">
          <h2>Galeria</h2>
          <div class="gallery" id="gallery-grid"></div>
        </div>

        <div id="contact-view" style="display:none">
          <h2>Kontakt</h2>
          <div style="display:grid;grid-template-columns:1fr 320px;gap:18px">
            <div class="card">
              <form id="contact-form">
                <label>Imię</label>
                <input required name="name" />
                <label>Email</label>
                <input required name="email" type="email" />
                <label>Wiadomość</label>
                <textarea required name="message" rows="6"></textarea>
                <div style="margin-top:10px"><button type="submit">Wyślij</button></div>
              </form>
            </div>

            <aside>
              <div class="card">
                <strong>Adres</strong>
                <div style="color:var(--muted);margin-top:8px">Przykładowa ulica 12, Miasto</div>
                <div style="margin-top:10px"><a href="#" style="color:var(--accent);text-decoration:none">Pokaż na mapie</a></div>
              </div>
              <div class="card">
                <strong>Subskrypcja</strong>
                <div style="margin-top:8px"><input placeholder="Twój email" /><div style="margin-top:8px"><button>Dołącz</button></div></div>
              </div>
            </aside>
          </div>
        </div>
      </main>

      <aside>
        <div class="card">
          <strong>O mnie</strong>
          <p style="color:var(--muted);margin-top:8px">Krótki opis autora bloga. Dodaj tu informacje o sobie i linki do mediów społecznościowych.</p>
        </div>

        <div class="card">
          <strong>Najpopularniejsze</strong>
          <div id="popular-list" style="margin-top:8px"></div>
        </div>

        <div class="card">
          <strong>Mini galeria</strong>
          <div class="gallery" id="side-gallery"></div>
        </div>
      </aside>
    </div>
  </div>

  <div class="modal" id="lightbox">
    <div class="inner">
      <button id="close-modal" style="float:right;border:0;background:#eee;padding:6px;border-radius:8px;cursor:pointer">Zamknij</button>
      <div style="clear:both"></div>
      <img id="lightbox-img" src="" alt=""/>
    </div>
  </div>

  <script>
    /* Dane przykładowe - edytuj według potrzeb */
    const posts = [
      {id:1,title:"Inspiracje do nowoczesnego wnętrza",date:"2025-10-01",category:"Design",excerpt:"Kilka prostych trików, które odmienią pokój.",image:"assets/images/gallery-1.jpg",content:"Pełna treść wpisu o wnętrzach. Dodaj więcej paragrafów, zdjęć i cytatów."},
      {id:2,title:"Fotograficzne porady dla początkujących",date:"2025-09-21",category:"Fotografia",excerpt:"Jak lepiej komponować zdjęcia i używać światła.",image:"assets/images/gallery-2.jpg",content:"Pełna treść wpisu o fotografii z przykładami i wskazówkami."},
      {id:3,title:"Kolory roku i jak je stosować",date:"2025-08-10",category:"Design",excerpt:"Jasnozielone akcenty w praktyce.",image:"assets/images/gallery-3.jpg",content:"Pełna treść wpisu o kolorystyce i paletach barw."}
    ];

    const galleryImages = [
      "assets/images/gallery-1.jpg",
      "assets/images/gallery-2.jpg",
      "assets/images/gallery-3.jpg",
      "assets/images/gallery-4.jpg",
      "assets/images/gallery-5.jpg",
      "assets/images/gallery-6.jpg"
    ];

    /* Inicjalizacja UI */
    const postsList = document.getElementById('posts-list');
    const postsCount = document.getElementById('posts-count');
    const categoryFilter = document.getElementById('category-filter');
    const searchInput = document.getElementById('search');
    const postView = document.getElementById('post-view');
    const homeView = document.getElementById('home-view');
    const galleryView = document.getElementById('gallery-view');
    const contactView = document.getElementById('contact-view');
    const galleryGrid = document.getElementById('gallery-grid');
    const sideGallery = document.getElementById('side-gallery');
    const popularList = document.getElementById('popular-list');
    const lightbox = document.getElementById('lightbox');
    const lightboxImg = document.getElementById('lightbox-img');

    function renderPosts(filterText='',category=''){
      const filtered = posts.filter(p=>{
        const matchesText = (p.title + p.excerpt + p.content).toLowerCase().includes(filterText.toLowerCase());
        const matchesCategory = category ? p.category === category : true;
        return matchesText && matchesCategory;
      });
      postsList.innerHTML = '';
      filtered.forEach(p=>{
        const el = document.createElement('article');
        el.className='post';
        el.innerHTML = \`
          <img src="\${p.image}" alt="">
          <div>
            <div class="meta">\${p.date} • \${p.category}</div>
            <h3>\${p.title}</h3>
            <p style="color:var(--muted)">\${p.excerpt}</p>
            <div style="margin-top:auto"><button data-id="\${p.id}" class="open-post">Czytaj</button></div>
          </div>
        \`;
        postsList.appendChild(el);
      });
      postsCount.textContent = filtered.length;
      // popular
      popularList.innerHTML = posts.slice(0,3).map(p=>'<div style="margin-bottom:8px"><strong>'+p.title+'</strong><div style="color:var(--muted);font-size:13px">'+p.date+'</div></div>').join('');
    }

    function populateCategories(){
      const cats = Array.from(new Set(posts.map(p=>p.category)));
      cats.forEach(c=>{
        const opt = document.createElement('option');
        opt.value = c; opt.textContent = c;
        categoryFilter.appendChild(opt);
      });
    }

    function renderGallery(){
      galleryGrid.innerHTML = galleryImages.map(src=>'<img src="'+src+'" alt="">').join('');
      sideGallery.innerHTML = galleryImages.slice(0,3).map(src=>'<img src="'+src+'" alt="">').join('');
      // event delegation
      galleryGrid.querySelectorAll('img').forEach(img=>{
        img.addEventListener('click',()=>openLightbox(img.src));
      });
      sideGallery.querySelectorAll('img').forEach(img=>{
        img.addEventListener('click',()=>openLightbox(img.src));
      });
    }

    function openLightbox(src){
      lightboxImg.src = src;
      lightbox.classList.add('open');
    }
    document.getElementById('close-modal').addEventListener('click',()=>lightbox.classList.remove('open'));
    lightbox.addEventListener('click',e=>{ if(e.target===lightbox) lightbox.classList.remove('open') });

    // nawigacja widoków
    document.querySelectorAll('nav a').forEach(a=>{
      a.addEventListener('click',e=>{
        e.preventDefault();
        const target = a.dataset.nav;
        showView(target);
      });
    });

    function showView(name){
      homeView.style.display = name==='home' || !name ? 'block' : 'none';
      galleryView.style.display = name==='gallery' ? 'block' : 'none';
      contactView.style.display = name==='contact' ? 'block' : 'none';
      postView.style.display = 'none';
    }

    // otwieranie wpisu
    document.addEventListener('click',e=>{
      if(e.target.classList.contains('open-post')){
        const id = Number(e.target.dataset.id);
        const post = posts.find(p=>p.id===id);
        if(post) showPost(post);
      }
    });

    function showPost(post){
      homeView.style.display = 'none';
      galleryView.style.display = 'none';
      contactView.style.display = 'none';
      postView.style.display = 'block';
      postView.innerHTML = \`
        <button id="back" style="margin-bottom:12px;padding:8px;border-radius:8px;border:1px solid #eee">Powrót</button>
        <div class="post-view-card">
          <div class="post-view-image"><img src="\${post.image}" alt=""></div>
          <h2>\${post.title}</h2>
          <div class="meta">\${post.date} • \${post.category}</div>
          <p style="color:var(--muted)">\${post.content}</p>
        </div>
      \`;
      document.getElementById('back').addEventListener('click',()=>showView('home'));
    }

    // filtrowanie i wyszukiwanie
    searchInput.addEventListener('input',()=>renderPosts(searchInput.value,categoryFilter.value));
    categoryFilter.addEventListener('change',()=>renderPosts(searchInput.value,categoryFilter.value));

    // formularz kontaktowy - tu trzeba dodać backend lub integrację
    document.getElementById('contact-form').addEventListener('submit',e=>{
      e.preventDefault();
      alert('Formularz wysłany lokalnie. Podłącz backend aby obsłużyć wiadomości.');
      e.target.reset();
    });

    // inicjalizacja
    populateCategories();
    renderPosts();
    renderGallery();
    showView('home');
  </script>
</body>
</html>


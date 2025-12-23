<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>.:BizlyGenTech:. Website Generator</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: -apple-system, sans-serif; background: #020617; color: #fff; }
        .cyber-grid {
            position: fixed; top: -50px; left: -50px; width: calc(100% + 100px); height: calc(100% + 100px);
            background: linear-gradient(90deg, rgba(168,85,247,0.8) 2px, transparent 2px), linear-gradient(rgba(168,85,247,0.8) 2px, transparent 2px);
            background-size: 50px 50px; z-index: -1; animation: gridMove 4s linear infinite; opacity: 0.2;
        }
        @keyframes gridMove { 0% { background-position: 0 0; } 100% { background-position: 50px 50px; } }
        .container { display: flex; height: 100vh; }
        .panel { width: 50%; padding: 2rem; overflow-y: auto; }
        .left { border-right: 1px solid rgba(6,182,212,0.3); background: linear-gradient(to bottom, #020617, #0f172a); }
        .right { background: #fff; }
        .title { font-size: 2rem; font-weight: 900; background: linear-gradient(to right, #22d3ee, #a855f7, #ec4899); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .btn { padding: 0.75rem 1.5rem; border: none; border-radius: 0.5rem; cursor: pointer; font-weight: 600; }
        .btn-primary { background: #22d3ee; color: #fff; }
        .btn-success { background: #22c55e; color: #fff; }
        .template-card { padding: 1rem; border: 2px solid #334155; border-radius: 0.5rem; margin: 0.5rem 0; cursor: pointer; }
        .template-card:hover { border-color: #22d3ee; }
        .template-card.selected { border-color: #22d3ee; background: rgba(6,182,212,0.2); }
        input[type="text"], input[type="email"] { width: 100%; padding: 0.5rem; background: #0f172a; border: 1px solid rgba(6,182,212,0.3); border-radius: 0.5rem; color: #fff; margin: 0.5rem 0; }
        input[type="color"] { width: 100%; height: 2.5rem; border-radius: 0.5rem; cursor: pointer; }
        .section-item { display: flex; justify-content: space-between; padding: 0.75rem; background: #1e293b; border-radius: 0.5rem; margin: 0.5rem 0; }
        .preview { width: 100%; height: 100%; border: none; }
        h2 { color: #22d3ee; margin: 1rem 0; }
        label { display: block; margin: 0.5rem 0; font-size: 0.875rem; }
    </style>
</head>
<body>
    <div class="cyber-grid"></div>
    <div class="container">
        <div class="panel left">
            <div class="title">CYBERSPACE</div>
            <p style="color: rgba(6,182,212,0.8); font-size: 0.875rem; margin-bottom: 2rem;">Website Generator</p>
            
            <div id="content">
                <h2>Choose Template</h2>
                <div class="template-card selected" onclick="selectTemplate('cyber')">
                    <div style="font-weight: bold; color: #67e8f9;">Cyber Neon</div>
                    <div style="font-size: 0.875rem; color: #94a3b8;">Futuristic purple grid</div>
                </div>
                <div class="template-card" onclick="selectTemplate('matrix')">
                    <div style="font-weight: bold; color: #67e8f9;">Matrix Code</div>
                    <div style="font-size: 0.875rem; color: #94a3b8;">Green terminal style</div>
                </div>
                
                <h2>Customize Content</h2>
                <label>Headline</label>
                <input type="text" id="headline" value="Welcome to the Future" onchange="updatePreview()">
                
                <label>Subheadline</label>
                <input type="text" id="subheadline" value="Next-generation technology solutions" onchange="updatePreview()">
                
                <label>Button Text</label>
                <input type="text" id="ctaText" value="Enter the Matrix" onchange="updatePreview()">
                
                <label>Primary Color</label>
                <input type="color" id="primaryColor" value="#00ffff" onchange="updatePreview()">
                
                <label>Contact Email</label>
                <input type="email" id="email" value="hello@startup.com" onchange="updatePreview()">
                
                <button class="btn btn-success" onclick="downloadHTML()" style="width: 100%; margin-top: 2rem;">Download HTML</button>
            </div>
        </div>
        
        <div class="panel right">
            <iframe id="preview" class="preview"></iframe>
        </div>
    </div>

    <script>
        let template = 'cyber';
        
        function selectTemplate(t) {
            template = t;
            document.querySelectorAll('.template-card').forEach(c => c.classList.remove('selected'));
            event.target.closest('.template-card').classList.add('selected');
            updatePreview();
        }
        
        function updatePreview() {
            const iframe = document.getElementById('preview');
            iframe.srcdoc = generateHTML();
        }
        
        function generateHTML() {
            const gridColor = template === 'matrix' ? 'rgba(0,255,0,0.8)' : 'rgba(168,85,247,0.8)';
            const primary = document.getElementById('primaryColor').value;
            const headline = document.getElementById('headline').value;
            const subheadline = document.getElementById('subheadline').value;
            const ctaText = document.getElementById('ctaText').value;
            const email = document.getElementById('email').value;
            
            return `<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>.:BizlyGenTech:.</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: -apple-system, sans-serif;
            background: #0a0e27;
            color: #e0e7ff;
            line-height: 1.6;
        }
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, ${gridColor} 2px, transparent 2px), linear-gradient(${gridColor} 2px, transparent 2px);
            background-size: 50px 50px;
            animation: gridMove 4s linear infinite;
            opacity: 0.2;
            z-index: -1;
        }
        @keyframes gridMove { 0% { background-position: 0 0; } 100% { background-position: 50px 50px; } }
        header {
            padding: 20px;
            background: rgba(10,14,39,0.95);
            border-bottom: 2px solid ${primary}44;
            position: sticky;
            top: 0;
        }
        .logo {
            font-size: 1.5rem;
            font-weight: 900;
            background: linear-gradient(135deg, ${primary}, #ff00ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            letter-spacing: 3px;
            text-transform: uppercase;
        }
        .container { max-width: 1200px; margin: 0 auto; padding: 0 20px; }
        .hero {
            padding: 120px 20px;
            text-align: center;
            background: linear-gradient(135deg, #0a0e27, ${primary}22);
        }
        h1 {
            font-size: 3.5rem;
            margin-bottom: 20px;
            background: linear-gradient(135deg, ${primary}, #ff00ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .hero p { font-size: 1.5rem; margin-bottom: 40px; opacity: 0.9; }
        .cta-button {
            display: inline-block;
            padding: 18px 45px;
            background: linear-gradient(135deg, ${primary}, #ff00ff);
            color: #0a0e27;
            text-decoration: none;
            border-radius: 12px;
            font-weight: 700;
            font-size: 1.1rem;
        }
        .features {
            padding: 100px 20px;
            background: #0a0e27;
        }
        h2 {
            text-align: center;
            font-size: 2.5rem;
            margin-bottom: 60px;
            color: ${primary};
        }
        .feature-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 40px;
        }
        .feature-card {
            padding: 40px;
            background: linear-gradient(135deg, ${primary}11, #ff00ff11);
            border-radius: 16px;
            border: 1px solid ${primary}44;
        }
        .feature-card h3 {
            color: ${primary};
            margin-bottom: 15px;
            font-size: 1.5rem;
        }
        .contact {
            padding: 100px 20px;
            text-align: center;
            background: #0a0e27;
        }
        .contact a {
            color: ${primary};
            font-size: 1.3rem;
            text-decoration: none;
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <div class="logo">.:BizlyGenTech:.</div>
        </div>
    </header>
    
    <section class="hero">
        <div class="container">
            <h1>${headline}</h1>
            <p>${subheadline}</p>
            <a href="#contact" class="cta-button">${ctaText}</a>
        </div>
    </section>
    
    <section class="features">
        <div class="container">
            <h2>Our Capabilities</h2>
            <div class="feature-grid">
                <div class="feature-card">
                    <h3>Quantum Speed</h3>
                    <p>Lightning-fast performance</p>
                </div>
                <div class="feature-card">
                    <h3>Neural Security</h3>
                    <p>AI-powered protection</p>
                </div>
                <div class="feature-card">
                    <h3>Cloud Fusion</h3>
                    <p>Seamless integration</p>
                </div>
            </div>
        </div>
    </section>
    
    <section class="contact" id="contact">
        <div class="container">
            <h2>Connect With Us</h2>
            <a href="mailto:${email}">${email}</a>
        </div>
    </section>
</body>
</html>`;
        }
        
        function downloadHTML() {
            const html = generateHTML();
            const blob = new Blob([html], { type: 'text/html' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'bizlygentech-website.html';
            a.click();
        }
        
        updatePreview();
    </script>
</body>
</html>

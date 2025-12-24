import React, { useState, useEffect } from 'react';
import { Download, Copy, Eye, ArrowRight, ArrowLeft, Smartphone, Tablet, Monitor, Plus, Trash2, Undo, Redo, Info, Zap, Sparkles, Mail } from 'lucide-react';

const TechWebsiteGenerator = () => {
  const [step, setStep] = useState(0);
  const [showOnboarding, setShowOnboarding] = useState(true);
  const [template, setTemplate] = useState('cyber');
  const [logo, setLogo] = useState(null);
  const [previewDevice, setPreviewDevice] = useState('desktop');
  const [history, setHistory] = useState([]);
  const [historyIndex, setHistoryIndex] = useState(-1);
  
  const [colorScheme, setColorScheme] = useState({
    primary: '#00ffff',
    secondary: '#ff00ff',
    accent: '#00ff00',
    background: '#0a0e27',
    text: '#e0e7ff'
  });
  
  const [sections, setSections] = useState([
    { id: 'hero', name: 'Hero Section', enabled: true, animation: 'fadeIn', data: {
      headline: 'Welcome to the Future',
      subheadline: 'Next-generation technology solutions',
      ctaText: 'Enter the Matrix',
      ctaLink: '#contact'
    }},
    { id: 'features', name: 'Features', enabled: true, animation: 'slideUp', data: {
      title: 'Our Capabilities',
      items: [
        { title: 'Quantum Speed', description: 'Lightning-fast performance' },
        { title: 'Neural Security', description: 'AI-powered protection' },
        { title: 'Cloud Fusion', description: 'Seamless integration' }
      ]
    }},
    { id: 'pricing', name: 'Pricing', enabled: true, animation: 'fadeIn', data: {
      title: 'Choose Your Plan',
      plans: [
        { name: 'Basic', price: '$29', features: ['5 Projects', 'Basic Support', '10GB Storage'] },
        { name: 'Pro', price: '$99', features: ['Unlimited Projects', 'Priority Support', '100GB Storage'] }
      ]
    }},
    { id: 'contact', name: 'Contact', enabled: true, animation: 'fadeIn', data: {
      title: 'Connect With Us',
      fields: [
        { type: 'text', label: 'Name', placeholder: 'Your name', required: true },
        { type: 'email', label: 'Email', placeholder: 'your@email.com', required: true },
        { type: 'textarea', label: 'Message', placeholder: 'Your message...', required: true }
      ]
    }}
  ]);

  const [previewMode, setPreviewMode] = useState(false);

  const saveToHistory = (newSections) => {
    const newHistory = history.slice(0, historyIndex + 1);
    newHistory.push(JSON.parse(JSON.stringify(newSections)));
    setHistory(newHistory);
    setHistoryIndex(newHistory.length - 1);
  };

  const undo = () => {
    if (historyIndex > 0) {
      setHistoryIndex(historyIndex - 1);
      setSections(JSON.parse(JSON.stringify(history[historyIndex - 1])));
    }
  };

  const redo = () => {
    if (historyIndex < history.length - 1) {
      setHistoryIndex(historyIndex + 1);
      setSections(JSON.parse(JSON.stringify(history[historyIndex + 1])));
    }
  };

  useEffect(() => {
    if (history.length === 0) {
      saveToHistory(sections);
    }
  }, []);

  const templates = {
    cyber: { name: 'Cyber Neon', desc: 'Futuristic with glowing effects' },
    matrix: { name: 'Matrix Code', desc: 'Green terminal aesthetic' },
    synthwave: { name: 'Synthwave', desc: 'Retro 80s vibes' }
  };

  const availableSections = [
    { id: 'team', name: 'Team Section', icon: '👥', desc: 'Showcase your team members' },
    { id: 'testimonials', name: 'Testimonials', icon: '💬', desc: 'Customer reviews' },
    { id: 'stats', name: 'Statistics', icon: '📊', desc: 'Key metrics and numbers' },
    { id: 'faq', name: 'FAQ', icon: '❓', desc: 'Frequently asked questions' },
    { id: 'gallery', name: 'Gallery', icon: '🖼️', desc: 'Image showcase' },
    { id: 'cta', name: 'Call to Action', icon: '🎯', desc: 'Conversion section' }
  ];

  const animations = ['fadeIn', 'slideUp', 'slideLeft', 'slideRight', 'zoomIn', 'none'];

  const tooltips = {
    template: 'Choose a visual style that matches your brand identity',
    logo: 'Upload your company logo (PNG, JPG, or SVG recommended)',
    colors: 'Customize your brand colors - changes apply in real-time',
    sections: 'Add or remove sections to build your perfect page',
    animation: 'Add motion to make your site more engaging',
    preview: 'Switch between devices to see how your site looks everywhere',
    export: 'Download or copy your complete website code'
  };

  const Tooltip = ({ text, children }) => (
    <div className="relative group">
      {children}
      <div className="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-2 px-3 py-2 bg-slate-900 text-xs text-cyan-300 rounded-lg opacity-0 group-hover:opacity-100 transition-opacity duration-300 whitespace-nowrap pointer-events-none border border-cyan-500/30 shadow-lg shadow-cyan-500/20 z-50">
        {text}
        <div className="absolute top-full left-1/2 transform -translate-x-1/2 border-4 border-transparent border-t-slate-900"></div>
      </div>
    </div>
  );

  const handleLogoUpload = (e) => {
    const file = e.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => setLogo(reader.result);
      reader.readAsDataURL(file);
    }
  };

  const updateSectionData = (sectionId, field, value) => {
    const newSections = sections.map(s => 
      s.id === sectionId ? { ...s, data: { ...s.data, [field]: value } } : s
    );
    setSections(newSections);
    saveToHistory(newSections);
  };

  const updateSectionAnimation = (sectionId, animation) => {
    const newSections = sections.map(s => 
      s.id === sectionId ? { ...s, animation } : s
    );
    setSections(newSections);
    saveToHistory(newSections);
  };

  const addSection = (sectionId) => {
    const sectionTemplate = availableSections.find(s => s.id === sectionId);
    const newSection = {
      id: sectionId,
      name: sectionTemplate.name,
      enabled: true,
      animation: 'fadeIn',
      data: sectionId === 'team' ? {
        title: 'Meet Our Team',
        members: [
          { name: 'Jane Doe', role: 'CEO', bio: 'Visionary leader' }
        ]
      } : sectionId === 'testimonials' ? {
        title: 'What Our Clients Say',
        testimonials: [
          { name: 'John Smith', company: 'Tech Corp', text: 'Amazing service!' }
        ]
      } : sectionId === 'stats' ? {
        stats: [
          { number: '10K+', label: 'Users' },
          { number: '99%', label: 'Uptime' }
        ]
      } : sectionId === 'faq' ? {
        title: 'FAQ',
        questions: [
          { q: 'How does it work?', a: 'It is simple and intuitive.' }
        ]
      } : sectionId === 'gallery' ? {
        title: 'Gallery',
        images: []
      } : { title: 'Ready to Get Started?', ctaText: 'Contact Us', ctaLink: '#contact' }
    };
    const newSections = [...sections, newSection];
    setSections(newSections);
    saveToHistory(newSections);
  };

  const removeSection = (sectionId) => {
    const newSections = sections.filter(s => s.id !== sectionId);
    setSections(newSections);
    saveToHistory(newSections);
  };

  const generateHTML = () => {
    const enabledSections = sections.filter(s => s.enabled);
    
    return `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cyberspace Tech</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Space Grotesk', -apple-system, BlinkMacSystemFont, sans-serif;
            background: ${colorScheme.background};
            color: ${colorScheme.text};
            line-height: 1.6;
            overflow-x: hidden;
        }
        
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                linear-gradient(90deg, ${colorScheme.primary}22 1px, transparent 1px),
                linear-gradient(${colorScheme.secondary}22 1px, transparent 1px);
            background-size: 50px 50px;
            opacity: 0.1;
            z-index: -1;
        }
        
        .container { max-width: 1200px; margin: 0 auto; padding: 0 20px; }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        @keyframes slideLeft {
            from { opacity: 0; transform: translateX(30px); }
            to { opacity: 1; transform: translateX(0); }
        }
        @keyframes slideRight {
            from { opacity: 0; transform: translateX(-30px); }
            to { opacity: 1; transform: translateX(0); }
        }
        @keyframes zoomIn {
            from { opacity: 0; transform: scale(0.9); }
            to { opacity: 1; transform: scale(1); }
        }
        @keyframes glow {
            0%, 100% { text-shadow: 0 0 20px ${colorScheme.primary}, 0 0 40px ${colorScheme.primary}; }
            50% { text-shadow: 0 0 30px ${colorScheme.primary}, 0 0 60px ${colorScheme.primary}; }
        }
        
        .animate-fadeIn { animation: fadeIn 1s ease-out; }
        .animate-slideUp { animation: slideUp 0.8s ease-out; }
        .animate-slideLeft { animation: slideLeft 0.8s ease-out; }
        .animate-slideRight { animation: slideRight 0.8s ease-out; }
        .animate-zoomIn { animation: zoomIn 0.8s ease-out; }
        
        header {
            padding: 20px 0;
            background: rgba(10, 14, 39, 0.95);
            backdrop-filter: blur(10px);
            position: sticky;
            top: 0;
            z-index: 100;
            border-bottom: 2px solid ${colorScheme.primary}44;
        }
        .logo {
            font-size: 24px;
            font-weight: bold;
            background: linear-gradient(135deg, ${colorScheme.primary}, ${colorScheme.secondary});
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: glow 3s infinite;
            letter-spacing: 3px;
            font-family: 'Orbitron', 'Courier New', monospace;
            text-transform: uppercase;
        }
        
        .hero {
            padding: 120px 20px;
            text-align: center;
            background: linear-gradient(135deg, ${colorScheme.background}, ${colorScheme.primary}22);
            position: relative;
            overflow: hidden;
        }
        .hero::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 500px;
            height: 500px;
            background: radial-gradient(circle, ${colorScheme.primary}33, transparent);
            transform: translate(-50%, -50%);
            border-radius: 50%;
            filter: blur(60px);
        }
        .hero h1 {
            font-size: 4rem;
            margin-bottom: 20px;
            font-weight: 900;
            background: linear-gradient(135deg, ${colorScheme.primary}, ${colorScheme.secondary});
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            position: relative;
        }
        .hero p {
            font-size: 1.5rem;
            margin-bottom: 40px;
            opacity: 0.9;
            position: relative;
        }
        .cta-button {
            display: inline-block;
            padding: 18px 45px;
            background: linear-gradient(135deg, ${colorScheme.primary}, ${colorScheme.secondary});
            color: ${colorScheme.background};
            text-decoration: none;
            border-radius: 12px;
            font-weight: 700;
            font-size: 1.1rem;
            transition: all 0.3s;
            position: relative;
            box-shadow: 0 10px 30px ${colorScheme.primary}66;
        }
        .cta-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 40px ${colorScheme.primary}99;
        }
        
        .features {
            padding: 100px 20px;
            background: ${colorScheme.background};
        }
        .features h2 {
            text-align: center;
            font-size: 3rem;
            margin-bottom: 60px;
            background: linear-gradient(135deg, ${colorScheme.primary}, ${colorScheme.accent});
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .feature-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 40px;
        }
        .feature-card {
            padding: 40px;
            background: linear-gradient(135deg, ${colorScheme.primary}11, ${colorScheme.secondary}11);
            border-radius: 16px;
            border: 1px solid ${colorScheme.primary}44;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }
        .feature-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, ${colorScheme.primary}22, transparent);
            opacity: 0;
            transition: opacity 0.3s;
        }
        .feature-card:hover {
            transform: translateY(-5px);
            border-color: ${colorScheme.primary};
            box-shadow: 0 20px 40px ${colorScheme.primary}44;
        }
        .feature-card:hover::before { opacity: 1; }
        .feature-card h3 {
            color: ${colorScheme.primary};
            margin-bottom: 15px;
            font-size: 1.8rem;
            position: relative;
        }
        .feature-card p { position: relative; }
        
        .pricing {
            padding: 100px 20px;
            background: linear-gradient(180deg, ${colorScheme.background}, ${colorScheme.primary}11);
        }
        .pricing h2 {
            text-align: center;
            font-size: 3rem;
            margin-bottom: 60px;
            color: ${colorScheme.primary};
        }
        .pricing-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 40px;
            max-width: 900px;
            margin: 0 auto;
        }
        .pricing-card {
            padding: 50px 40px;
            background: linear-gradient(135deg, ${colorScheme.primary}11, ${colorScheme.secondary}11);
            border-radius: 20px;
            border: 2px solid ${colorScheme.primary};
            text-align: center;
            transition: all 0.3s;
            position: relative;
        }
        .pricing-card:hover {
            transform: scale(1.05);
            box-shadow: 0 30px 60px ${colorScheme.primary}55;
        }
        .pricing-card h3 {
            font-size: 2rem;
            margin-bottom: 10px;
            color: ${colorScheme.primary};
        }
        .price {
            font-size: 4rem;
            color: ${colorScheme.accent};
            margin: 30px 0;
            font-weight: 900;
        }
        .features-list {
            list-style: none;
            margin: 30px 0;
        }
        .features-list li {
            padding: 12px 0;
            border-bottom: 1px solid ${colorScheme.primary}33;
        }
        
        .contact {
            padding: 100px 20px;
            background: ${colorScheme.background};
        }
        .contact h2 {
            text-align: center;
            font-size: 3rem;
            margin-bottom: 60px;
            color: ${colorScheme.primary};
        }
        .contact-form {
            max-width: 600px;
            margin: 0 auto;
        }
        .form-group {
            margin-bottom: 25px;
        }
        .form-group label {
            display: block;
            margin-bottom: 8px;
            color: ${colorScheme.primary};
            font-weight: 600;
        }
        .form-group input,
        .form-group textarea {
            width: 100%;
            padding: 15px;
            background: ${colorScheme.primary}11;
            border: 2px solid ${colorScheme.primary}44;
            border-radius: 8px;
            color: ${colorScheme.text};
            font-size: 16px;
            transition: all 0.3s;
        }
        .form-group input:focus,
        .form-group textarea:focus {
            outline: none;
            border-color: ${colorScheme.primary};
            box-shadow: 0 0 20px ${colorScheme.primary}44;
        }
        .form-group textarea {
            min-height: 150px;
            resize: vertical;
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <div class="logo">${logo ? `<img src="${logo}" alt="Logo" style="height: 40px;">` : 'BizleyGenTech'}</div>
        </div>
    </header>
    
    ${enabledSections.map(section => {
      const animClass = section.animation !== 'none' ? `animate-${section.animation}` : '';
      
      if (section.id === 'hero') {
        return `<section class="hero ${animClass}">
        <div class="container">
            <h1>${section.data.headline}</h1>
            <p>${section.data.subheadline}</p>
            <a href="${section.data.ctaLink}" class="cta-button">${section.data.ctaText}</a>
        </div>
    </section>`;
      }
      if (section.id === 'features') {
        return `<section class="features ${animClass}">
        <div class="container">
            <h2>${section.data.title}</h2>
            <div class="feature-grid">
                ${section.data.items.map(item => `
                <div class="feature-card">
                    <h3>${item.title}</h3>
                    <p>${item.description}</p>
                </div>`).join('')}
            </div>
        </div>
    </section>`;
      }
      if (section.id === 'pricing') {
        return `<section class="pricing ${animClass}">
        <div class="container">
            <h2>${section.data.title}</h2>
            <div class="pricing-grid">
                ${section.data.plans.map(plan => `
                <div class="pricing-card">
                    <h3>${plan.name}</h3>
                    <div class="price">${plan.price}</div>
                    <ul class="features-list">
                        ${plan.features.map(f => `<li>${f}</li>`).join('')}
                    </ul>
                    <a href="#contact" class="cta-button">Choose Plan</a>
                </div>`).join('')}
            </div>
        </div>
    </section>`;
      }
      if (section.id === 'contact') {
        return `<section class="contact ${animClass}">
        <div class="container">
            <h2>${section.data.title}</h2>
            <form class="contact-form">
                ${section.data.fields.map(field => `
                <div class="form-group">
                    <label>${field.label}${field.required ? ' *' : ''}</label>
                    ${field.type === 'textarea' 
                      ? `<textarea placeholder="${field.placeholder}" ${field.required ? 'required' : ''}></textarea>`
                      : `<input type="${field.type}" placeholder="${field.placeholder}" ${field.required ? 'required' : ''}>`
                    }
                </div>`).join('')}
                <button type="submit" class="cta-button">Send Message</button>
            </form>
        </div>
    </section>`;
      }
      return '';
    }).join('\n    ')}
</body>
</html>`;
  };

  const downloadHTML = () => {
    const html = generateHTML();
    const blob = new Blob([html], { type: 'text/html' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'cyberspace-website.html';
    a.click();
  };

  const copyToClipboard = () => {
    navigator.clipboard.writeText(generateHTML());
    alert('Code copied to clipboard!');
  };

  const steps = [
    { name: 'Template', icon: Sparkles, desc: 'Choose your style' },
    { name: 'Branding', icon: Zap, desc: 'Colors and logo' },
    { name: 'Sections', icon: Plus, desc: 'Build your page' },
    { name: 'Customize', icon: Info, desc: 'Edit content' },
    { name: 'Export', icon: Download, desc: 'Get your site' }
  ];

  const deviceSizes = {
    desktop: { width: '100%', icon: Monitor },
    tablet: { width: '768px', icon: Tablet },
    mobile: { width: '375px', icon: Smartphone }
  };

  return (
    <div className="min-h-screen bg-slate-950 text-white relative overflow-hidden">
      <div className="fixed inset-0 opacity-20 pointer-events-none">
        <div className="absolute inset-0" style={{
          background: 'repeating-linear-gradient(0deg, cyan 0px, cyan 1px, transparent 1px, transparent 50px), repeating-linear-gradient(90deg, magenta 0px, magenta 1px, transparent 1px, transparent 50px)'
        }}></div>
      </div>

      {showOnboarding && (
        <div className="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 flex items-center justify-center p-4">
          <div className="bg-gradient-to-br from-slate-900 to-slate-800 p-8 rounded-2xl max-w-md border-2 border-cyan-500 shadow-2xl shadow-cyan-500/50">
            <h2 className="text-3xl font-bold mb-4 bg-gradient-to-r from-cyan-400 to-purple-500 bg-clip-text text-transparent">
              Welcome to Cyberspace Generator!
            </h2>
            <p className="text-slate-300 mb-6">
              Create stunning tech websites in 5 easy steps. Use tooltips (hover over info icons) for help. Your changes are saved automatically with undo/redo support!
            </p>
            <button
              onClick={() => setShowOnboarding(false)}
              className="w-full py-3 bg-gradient-to-r from-cyan-500 to-purple-500 hover:from-cyan-600 hover:to-purple-600 rounded-lg font-bold transition-all transform hover:scale-105"
            >
              Let's Build!
            </button>
          </div>
        </div>
      )}

      {!previewMode ? (
        <div className="flex h-screen">
          <div className="w-1/2 border-r border-cyan-500/30 overflow-y-auto bg-gradient-to-b from-slate-950 to-slate-900">
            <div className="p-8">
              <div className="flex items-center justify-between mb-6">
                <div>
                  <h1 className="text-4xl font-black mb-2 bg-gradient-to-r from-cyan-400 via-purple-500 to-pink-500 bg-clip-text text-transparent">
                    CYBERSPACE
                  </h1>
                  <p className="text-cyan-400/80 text-sm">Website Generator v2.0</p>
                </div>
                <div className="flex gap-2">
                  <Tooltip text="Undo last change">
                    <button
                      onClick={undo}
                      disabled={historyIndex <= 0}
                      className="p-3 bg-slate-800 hover:bg-slate-700 disabled:opacity-30 disabled:cursor-not-allowed rounded-lg border border-cyan-500/30 transition-all"
                    >
                      <Undo size={20} className="text-cyan-400" />
                    </button>
                  </Tooltip>
                  <Tooltip text="Redo last change">
                    <button
                      onClick={redo}
                      disabled={historyIndex >= history.length - 1}
                      className="p-3 bg-slate-800 hover:bg-slate-700 disabled:opacity-30 disabled:cursor-not-allowed rounded-lg border border-cyan-500/30 transition-all"
                    >
                      <Redo size={20} className="text-cyan-400" />
                    </button>
                  </Tooltip>
                </div>
              </div>

              <div className="flex items-center justify-between mb-8 bg-slate-900/50 p-4 rounded-xl border border-cyan-500/20">
                {steps.map((s, i) => {
                  const Icon = s.icon;
                  return (
                    <div key={i} className="flex items-center">
                      <Tooltip text={s.desc}>
                        <div className={`flex flex-col items-center cursor-pointer transition-all ${
                          i === step ? 'scale-110' : 'opacity-60'
                        }`} onClick={() => setStep(i)}>
                          <div className={`w-12 h-12 rounded-full flex items-center justify-center transition-all ${
                            i <= step 
                              ? 'bg-gradient-to-r from-cyan-500 to-purple-500 shadow-lg shadow-cyan-500/50' 
                              : 'bg-slate-800 border border-slate-700'
                          }`}>
                            <Icon size={20} />
                          </div>
                          <span className="text-xs mt-2">{s.name}</span>
                        </div>
                      </Tooltip>
                      {i < steps.length - 1 && (
                        <div className={`w-8 h-1 mx-2 transition-all ${
                          i < step 
                            ? 'bg-gradient-to-r from-cyan-500 to-purple-500' 
                            : 'bg-slate-800'
                        }`} />
                      )}
                    </div>
                  );
                })}
              </div>

              <div className="space-y-6">
                {step === 0 && (
                  <div>
                    <div className="flex items-center gap-2 mb-4">
                      <h2 className="text-2xl font-bold text-cyan-400">Choose Template</h2>
                      <Tooltip text={tooltips.template}>
                        <Info size={18} className="text-cyan-500 cursor-help" />
                      </Tooltip>
                    </div>
                    <div className="space-y-3">
                      {Object.entries(templates).map(([key, val]) => (
                        <div
                          key={key}
                          onClick={() => setTemplate(key)}
                          className={`p-6 rounded-xl border-2 cursor-pointer transition-all ${
                            template === key 
                              ? 'border-cyan-500 bg-gradient-to-r from-cyan-500/20 to-purple-500/20 shadow-lg shadow-cyan-500/30' 
                              : 'border-slate-700 hover:border-cyan-600 bg-slate-800/50'
                          }`}
                        >
                          <div className="font-bold text-lg text-cyan-300">{val.name}</div>
                          <div className="text-sm text-slate-400 mt-1">{val.desc}</div>
                        </div>
                      ))}
                    </div>
                  </div>
                )}

                {step === 1 && (
                  <div>
                    <div className="flex items-center gap-2 mb-4">
                      <h2 className="text-2xl font-bold text-cyan-400">Branding</h2>
                      <Tooltip text={tooltips.logo}>
                        <Info size={18} className="text-cyan-500 cursor-help" />
                      </Tooltip>
                    </div>
                    <div className="space-y-6">
                      <div className="p-6 bg-slate-800/50 rounded-xl border border-cyan-500/20">
                        <label className="block text-sm font-medium mb-3 text-cyan-300">Logo Upload</label>
                        <input
                          type="file"
                          accept="image/*"
                          onChange={handleLogoUpload}
                          className="block w-full text-sm text-slate-400 file:mr-4 file:py-3 file:px-6 file:rounded-lg file:border-0 file:bg-gradient-to-r file:from-cyan-500 file:to-purple-500 file:text-white hover:file:from-cyan-600 hover:file:to-purple-600 file:cursor-pointer file:transition-all"
                        />
                        {logo && (
                          <div className="mt-4 p-4 bg-slate-900 rounded-lg border border-cyan-500/30">
                            <img src={logo} alt="Logo preview" className="h-16 mx-auto" />
                          </div>
                        )}
                      </div>
                      <div className="p-6 bg-slate-800/50 rounded-xl border border-cyan-500/20">
                        <div className="flex items-center gap-2 mb-3">
                          <label className="block text-sm font-medium text-cyan-300">Color Scheme</label>
                          <Tooltip text={tooltips.colors}>
                            <Info size={16} className="text-cyan-500 cursor-help" />
                          </Tooltip>
                        </div>
                        <div className="grid grid-cols-2 gap-4">
                          {Object.entries(colorScheme).map(([key, value]) => (
                            <div key={key} className="space-y-2">
                              <label className="text-xs text-slate-400 uppercase tracking-wider">{key}</label>
                              <div className="flex gap-2 items-center">
                                <input
                                  type="color"
                                  value={value}
                                  onChange={(e) => setColorScheme({...colorScheme, [key]: e.target.value})}
                                  className="w-full h-12 rounded-lg cursor-pointer border-2 border-cyan-500/30"
                                />
                                <div className="text-xs text-slate-500 font-mono">{value}</div>
                              </div>
                            </div>
                          ))}
                        </div>
                      </div>
                    </div>
                  </div>
                )}

                {step === 2 && (
                  <div>
                    <div className="flex items-center gap-2 mb-4">
                      <h2 className="text-2xl font-bold text-cyan-400">Manage Sections</h2>
                      <Tooltip text={tooltips.sections}>
                        <Info size={18} className="text-cyan-500 cursor-help" />
                      </Tooltip>
                    </div>
                    <div className="space-y-4">
                      <div className="p-4 bg-slate-900/50 rounded-lg border border-cyan-500/20">
                        <h3 className="text-sm font-semibold text-cyan-300 mb-3">Active Sections</h3>
                        <div className="space-y-2">
                          {sections.map((section) => (
                            <div key={section.id} className="flex items-center justify-between p-4 bg-slate-800 rounded-lg border border-cyan-500/20 hover:border-cyan-500/50 transition-all">
                              <div>
                                <span className="font-medium text-slate-200">{section.name}</span>
                                <div className="text-xs text-slate-500 mt-1">Animation: {section.animation}</div>
                              </div>
                              <button
                                onClick={() => removeSection(section.id)}
                                className="p-2 hover:bg-red-500/20 rounded-lg transition-all group"
                              >
                                <Trash2 size={18} className="text-slate-400 group-hover:text-red-400" />
                              </button>
                            </div>
                          ))}
                        </div>
                      </div>
                      <div className="p-4 bg-slate-900/50 rounded-lg border border-cyan-500/20">
                        <h3 className="text-sm font-semibold text-cyan-300 mb-3">Add New Section</h3>
                        <div className="space-y-2">
                          {availableSections.filter(s => !sections.find(sec => sec.id === s.id)).map(section => (
                            <button
                              key={section.id}
                              onClick={() => addSection(section.id)}
                              className="w-full text-left p-4 bg-slate-800 hover:bg-slate-700 rounded-lg transition-all border border-transparent hover:border-cyan-500/30 group"
                            >
                              <div className="flex items-center gap-3">
                                <span className="text-2xl">{section.icon}</span>
                                <div>
                                  <div className="font-medium text-slate-200 group-hover:text-cyan-300 transition-colors">{section.name}</div>
                                  <div className="text-xs text-slate-500">{section.desc}</div>
                                </div>
                                <Plus size={18} className="ml-auto text-slate-500 group-hover:text-cyan-400" />
                              </div>
                            </button>
                          ))}
                        </div>
                      </div>
                    </div>
                  </div>
                )}

                {step === 3 && (
                  <div>
                    <div className="flex items-center gap-2 mb-4">
                      <h2 className="text-2xl font-bold text-cyan-400">Customize Content</h2>
                      <Tooltip text="Edit your section content and choose animations">
                        <Info size={18} className="text-cyan-500 cursor-help" />
                      </Tooltip>
                    </div>
                    <div className="space-y-6">
                      {sections.filter(s => s.enabled).map((section) => (
                        <div key={section.id} className="p-6 bg-slate-800/50 rounded-xl border border-cyan-500/20">
                          <h3 className="font-semibold text-cyan-300 mb-4 flex items-center gap-2">
                            {section.name}
                          </h3>
                          
                          <div className="mb-4">
                            <label className="block text-xs text-slate-400 mb-2">Section Animation</label>
                            <select
                              value={section.animation}
                              onChange={(e) => updateSectionAnimation(section.id, e.target.value)}
                              className="w-full p-3 bg-slate-900 rounded-lg border border-cyan-500/30 text-slate-200 focus:border-cyan-500 focus:outline-none"
                            >
                              {animations.map(anim => (
                                <option key={anim} value={anim}>{anim}</option>
                              ))}
                            </select>
                          </div>

                          {section.id === 'hero' && (
                            <div className="space-y-4">
                              <div>
                                <label className="block text-xs text-slate-400 mb-2">Headline</label>
                                <input
                                  type="text"
                                  placeholder="Headline"
                                  value={section.data.headline}
                                  onChange={(e) => updateSectionData('hero', 'headline', e.target.value)}
                                  className="w-full p-3 bg-slate-900 rounded-lg border border-cyan-500/30 text-slate-200 focus:border-cyan-500 focus:outline-none"
                                />
                              </div>
                              <div>
                                <label className="block text-xs text-slate-400 mb-2">Subheadline</label>
                                <input
                                  type="text"
                                  placeholder="Subheadline"
                                  value={section.data.subheadline}
                                  onChange={(e) => updateSectionData('hero', 'subheadline', e.target.value)}
                                  className="w-full p-3 bg-slate-900 rounded-lg border border-cyan-500/30 text-slate-200 focus:border-cyan-500 focus:outline-none"
                                />
                              </div>
                              <div>
                                <label className="block text-xs text-slate-400 mb-2">CTA Button Text</label>
                                <input
                                  type="text"
                                  placeholder="CTA Text"
                                  value={section.data.ctaText}
                                  onChange={(e) => updateSectionData('hero', 'ctaText', e.target.value)}
                                  className="w-full p-3 bg-slate-900 rounded-lg border border-cyan-500/30 text-slate-200 focus:border-cyan-500 focus:outline-none"
                                />
                              </div>
                            </div>
                          )}
                          {section.id === 'contact' && (
                            <div>
                              <label className="block text-xs text-slate-400 mb-2">Section Title</label>
                              <input
                                type="text"
                                placeholder="Contact Title"
                                value={section.data.title}
                                onChange={(e) => updateSectionData('contact', 'title', e.target.value)}
                                className="w-full p-3 bg-slate-900 rounded-lg border border-cyan-500/30 text-slate-200 focus:border-cyan-500 focus:outline-none"
                              />
                              <p className="text-xs text-slate-500 mt-2">Form fields can be customized in the form builder</p>
                            </div>
                          )}
                        </div>
                      ))}
                    </div>
                  </div>
                )}

                {step === 4 && (
                  <div>
                    <div className="flex items-center gap-2 mb-4">
                      <h2 className="text-2xl font-bold text-cyan-400">Export Your Website</h2>
                      <Tooltip text={tooltips.export}>
                        <Info size={18} className="text-cyan-500 cursor-help" />
                      </Tooltip>
                    </div>
                    <div className="space-y-4">
                      <button
                        onClick={() => setPreviewMode(true)}
                        className="w-full p-5 bg-gradient-to-r from-indigo-500 to-purple-500 hover:from-indigo-600 hover:to-purple-600 rounded-xl flex items-center justify-center gap-3 transition-all transform hover:scale-105 shadow-lg shadow-indigo-500/50"
                      >
                        <Eye size={22} />
                        <span className="font-bold text-lg">Full Screen Preview</span>
                      </button>
                      <button
                        onClick={downloadHTML}
                        className="w-full p-5 bg-gradient-to-r from-green-500 to-emerald-500 hover:from-green-600 hover:to-emerald-600 rounded-xl flex items-center justify-center gap-3 transition-all transform hover:scale-105 shadow-lg shadow-green-500/50"
                      >
                        <Download size={22} />
                        <span className="font-bold text-lg">Download HTML File</span>
                      </button>
                      <button
                        onClick={copyToClipboard}
                        className="w-full p-5 bg-gradient-to-r from-purple-500 to-pink-500 hover:from-purple-600 hover:to-pink-600 rounded-xl flex items-center justify-center gap-3 transition-all transform hover:scale-105 shadow-lg shadow-purple-500/50"
                      >
                        <Copy size={22} />
                        <span className="font-bold text-lg">Copy Code to Clipboard</span>
                      </button>
                      <div className="p-6 bg-slate-800/50 rounded-xl border border-cyan-500/20 mt-6">
                        <h3 className="font-semibold text-cyan-300 mb-2">What's Next?</h3>
                        <ul className="text-sm text-slate-400 space-y-2">
                          <li>✨ Download and open the HTML file in any browser</li>
                          <li>🎨 Further customize the code with your own styles</li>
                          <li>🚀 Deploy to any web hosting service</li>
                          <li>📱 Test on different devices for responsiveness</li>
                        </ul>
                      </div>
                    </div>
                  </div>
                )}
              </div>

              <div className="flex justify-between mt-8 pt-6 border-t border-cyan-500/20">
                <button
                  onClick={() => setStep(Math.max(0, step - 1))}
                  disabled={step === 0}
                  className="px-6 py-3 bg-slate-800 hover:bg-slate-700 disabled:opacity-30 disabled:cursor-not-allowed rounded-lg flex items-center gap-2 transition-all border border-cyan-500/20"
                >
                  <ArrowLeft size={20} />
                  Back
                </button>
                <button
                  onClick={() => setStep(Math.min(steps.length - 1, step + 1))}
                  disabled={step === steps.length - 1}
                  className="px-6 py-3 bg-gradient-to-r from-cyan-500 to-purple-500 hover:from-cyan-600 hover:to-purple-600 disabled:opacity-30 disabled:cursor-not-allowed rounded-lg flex items-center gap-2 transition-all shadow-lg shadow-cyan-500/30"
                >
                  Next
                  <ArrowRight size={20} />
                </button>
              </div>
            </div>
          </div>

          <div className="w-1/2 bg-white overflow-hidden flex flex-col">
            <div className="sticky top-0 bg-slate-900 p-4 border-b border-cyan-500/30 z-10 flex items-center justify-between">
              <h3 className="font-semibold text-cyan-300">Live Preview</h3>
              <div className="flex gap-2">
                <Tooltip text="Desktop view">
                  <button
                    onClick={() => setPreviewDevice('desktop')}
                    className={`p-2 rounded-lg transition-all ${
                      previewDevice === 'desktop'
                        ? 'bg-cyan-500 text-white'
                        : 'bg-slate-800 text-slate-400 hover:bg-slate-700'
                    }`}
                  >
                    <Monitor size={18} />
                  </button>
                </Tooltip>
                <Tooltip text="Tablet view">
                  <button
                    onClick={() => setPreviewDevice('tablet')}
                    className={`p-2 rounded-lg transition-all ${
                      previewDevice === 'tablet'
                        ? 'bg-cyan-500 text-white'
                        : 'bg-slate-800 text-slate-400 hover:bg-slate-700'
                    }`}
                  >
                    <Tablet size={18} />
                  </button>
                </Tooltip>
                <Tooltip text="Mobile view">
                  <button
                    onClick={() => setPreviewDevice('mobile')}
                    className={`p-2 rounded-lg transition-all ${
                      previewDevice === 'mobile'
                        ? 'bg-cyan-500 text-white'
                        : 'bg-slate-800 text-slate-400 hover:bg-slate-700'
                    }`}
                  >
                    <Smartphone size={18} />
                  </button>
                </Tooltip>
              </div>
            </div>
            <div className="flex-1 bg-slate-900 p-4 overflow-auto flex items-start justify-center">
              <div style={{ width: deviceSizes[previewDevice].width, minHeight: '100%' }} className="bg-white transition-all duration-300">
                <iframe
                  srcDoc={generateHTML()}
                  className="w-full h-full border-0"
                  style={{ minHeight: '600px' }}
                  title="preview"
                />
              </div>
            </div>
          </div>
        </div>
      ) : (
        <div className="relative">
          <button
            onClick={() => setPreviewMode(false)}
            className="fixed top-4 right-4 z-50 px-6 py-3 bg-gradient-to-r from-cyan-500 to-purple-500 hover:from-cyan-600 hover:to-purple-600 rounded-lg shadow-2xl shadow-cyan-500/50 font-bold transition-all transform hover:scale-105"
          >
            Back to Editor
          </button>
          <iframe
            srcDoc={generateHTML()}
            className="w-full h-screen border-0"
            title="full-preview"
          />
        </div>
      )}
    </div>
  );
};

export default TechWebsiteGenerator;

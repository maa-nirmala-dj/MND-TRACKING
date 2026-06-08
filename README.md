<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="theme-color" content="#050508">
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="mobile-web-app-capable" content="yes">
    
    <title>MND Live Logistics | Premium Cloud Tracking</title>

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600;800;900&family=Outfit:wght@300;400;500;700;900&family=Orbitron:wght@500;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <script src="https://www.gstatic.com/firebasejs/10.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.10.0/firebase-database-compat.js"></script>

    <style>
        /* =========================================================================
           GLOBAL RESET & VARIABLES
           ========================================================================= */
        :root {
            --gold: #D4AF37;
            --gold-glow: rgba(212, 175, 55, 0.4);
            --neon-green: #00FA9A;
            --neon-cyan: #00E5FF;
            --bg-dark: #050508;
            --danger: #ff3333;
            
            /* WhatsApp Dark Theme Colors */
            --wa-bg: #0b141a;
            --wa-header: #202c33;
            --wa-sent: #005c4b;
            --wa-received: #202c33;
            --wa-text: #e9edef;
            --wa-time: rgba(255,255,255,0.55);

            /* Modern Input Colors */
            --brand-purple: #7C3AED;
            --input-bg: #1f2937;
            --icon-gray: #9ca3af;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; outline: none; user-select: none; -webkit-user-select: none; }
        body, html { width: 100%; height: 100dvh; max-width: 100%; background: var(--bg-dark); color: #fff; overflow: hidden; display: flex; justify-content: center; align-items: flex-start; overscroll-behavior-y: none; }
        input, textarea { user-select: auto; -webkit-user-select: auto; }

        body.chat-locked { overflow: hidden !important; touch-action: none !important; position: fixed !important; width: 100% !important; height: 100% !important; }

        .bg-map {
            position: fixed; inset: 0; z-index: -1; opacity: 0.15;
            background-image: 
                radial-gradient(circle at 20% 30%, var(--gold-glow) 0%, transparent 50%),
                radial-gradient(circle at 80% 80%, rgba(0, 250, 154, 0.15) 0%, transparent 50%);
            animation: pulseBg 6s infinite alternate ease-in-out;
        }
        .grid-overlay {
            position: fixed; inset: 0; z-index: -1; opacity: 0.05;
            background-image: linear-gradient(var(--gold) 1px, transparent 1px), linear-gradient(90deg, var(--gold) 1px, transparent 1px);
            background-size: 40px 40px;
        }
        @keyframes pulseBg { 0% { opacity: 0.1; transform: scale(1); } 100% { opacity: 0.25; transform: scale(1.05); } }

        /* =========================================================================
           NATIVE-STYLE PUSH NOTIFICATIONS, TOASTS & INSTALL BANNER
           ========================================================================= */
        #toast-container { position: fixed; top: 15px; left: 50%; transform: translateX(-50%); z-index: 9999999; display: flex; flex-direction: column; gap: 10px; pointer-events: none; width: 100%; align-items: center; }
        .toast { background: rgba(10,10,15,0.98); border-left: 4px solid var(--gold); color: #fff; padding: 12px 18px; border-radius: 8px; font-weight: 600; font-size: 13px; box-shadow: 0 10px 30px rgba(0,0,0,0.8); display: flex; align-items: center; gap: 10px; max-width: 90%; animation: dropDown 0.4s cubic-bezier(0.2, 0.8, 0.2, 1) forwards, slideUp 0.4s 3.5s forwards; }
        .toast.success { border-color: var(--neon-green); }
        .toast.error { border-color: var(--danger); }
        
        .native-push { background: rgba(20, 20, 25, 0.95); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.15); border-radius: 18px; padding: 12px 15px; width: 92%; max-width: 400px; display: flex; gap: 12px; box-shadow: 0 20px 40px rgba(0,0,0,0.8), 0 0 20px rgba(212, 175, 55, 0.2); animation: dropDownPush 0.6s cubic-bezier(0.2, 0.8, 0.2, 1) forwards, slideUpPush 0.5s 6s forwards; pointer-events: auto; }
        .native-push-icon { width: 40px; height: 40px; min-width: 40px; border-radius: 10px; background: linear-gradient(135deg, var(--gold), #FFD700); display: flex; align-items: center; justify-content: center; color: #000; font-size: 20px; box-shadow: 0 5px 15px rgba(212,175,55,0.4); }
        .native-push-content { flex-grow: 1; display: flex; flex-direction: column; justify-content: center; overflow: hidden; }
        .native-push-title { font-family: 'Outfit', sans-serif; font-size: 14px; font-weight: 800; color: #fff; margin: 0 0 3px 0; text-transform: uppercase; letter-spacing: 1px; display:flex; justify-content:space-between; }
        .native-push-title span { font-size: 10px; color: var(--gold); font-weight: 500; text-transform: none; }
        .native-push-body { font-family: 'Outfit', sans-serif; font-size: 12px; color: #ccc; margin: 0; line-height: 1.4; font-weight: 500; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

        @keyframes dropDown { from { opacity: 0; transform: translateY(-30px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes slideUp { from { opacity: 1; transform: translateY(0); } to { opacity: 0; transform: translateY(-30px); } }
        @keyframes dropDownPush { from { opacity: 0; transform: translateY(-50px) scale(0.95); } to { opacity: 1; transform: translateY(0) scale(1); } }
        @keyframes slideUpPush { from { opacity: 1; transform: translateY(0) scale(1); } to { opacity: 0; transform: translateY(-50px) scale(0.95); } }

        .install-banner { position: fixed; top: 0; left: 0; right: 0; background: rgba(20,20,25,0.98); backdrop-filter: blur(15px); padding: 15px 20px; z-index: 99999999; display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid var(--gold); box-shadow: 0 10px 30px rgba(0,0,0,0.8); transform: translateY(-100%); transition: transform 0.4s cubic-bezier(0.2, 0.8, 0.2, 1); }
        .install-banner.show { transform: translateY(0); }

        /* =========================================================================
           VIEW MANAGER & FOOTER
           ========================================================================= */
        .view-container { display: none; flex-direction: column; align-items: center; width: 100%; height: 100dvh; max-width: 100vw; padding: 15px 12px 20px; animation: fadeInView 0.5s ease forwards; overflow-y: auto; overflow-x: hidden; -webkit-overflow-scrolling: touch; overscroll-behavior-y: none; }
        .active-view { display: flex !important; }
        @keyframes fadeInView { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }
        ::-webkit-scrollbar { width: 4px; } ::-webkit-scrollbar-track { background: transparent; } ::-webkit-scrollbar-thumb { background: rgba(212,175,55,0.4); border-radius: 10px; }

        .app-footer { width: 100%; text-align: center; font-family: 'Cinzel', serif; font-size: 10px; color: rgba(212, 175, 55, 0.6); margin-top: auto; padding-top: 30px; letter-spacing: 1px; font-weight: 700; flex-shrink: 0; transition: opacity 0.3s ease; }

        /* =========================================================================
           UI COMPONENTS (Inputs, Buttons, Cards)
           ========================================================================= */
        .app-title { font-family: 'Cinzel', serif; color: var(--gold); font-size: clamp(22px, 6vw, 28px); font-weight: 900; letter-spacing: 2px; margin-bottom: 5px; text-shadow: 0 0 15px var(--gold-glow); text-align: center; }
        .app-subtitle { color: #aaa; font-size: 12px; margin-bottom: 25px; line-height: 1.4; font-weight: 300; text-align: center; padding: 0 10px; }

        div[style*="display:flex"] { max-width: 100%; box-sizing: border-box; }
        div[style*="display:flex"] > input, div[style*="display:flex"] > .mn-input { min-width: 0; }

        .input-group { width: 100%; position: relative; margin-bottom: 15px; }
        .input-group i { position: absolute; left: 16px; top: 16px; color: var(--gold); font-size: 16px; opacity: 0.8; }
        .mn-input { width: 100%; padding: 14px 14px 14px 42px; background: rgba(0,0,0,0.6); border: 1px solid rgba(212,175,55,0.3); color: #fff; border-radius: 12px; font-size: 14px; outline: none; transition: 0.3s ease; box-shadow: inset 0 2px 10px rgba(0,0,0,0.5); box-sizing: border-box; }
        .mn-input:focus { border-color: var(--gold); background: rgba(212,175,55,0.05); box-shadow: 0 0 15px var(--gold-glow), inset 0 2px 10px rgba(0,0,0,0.5); }
        
        .pin-style { letter-spacing: 8px; font-size: 20px; font-weight: 900; text-align: center; padding-left: 15px; font-family: 'Orbitron', sans-serif; color: var(--neon-cyan); }
        .pin-style + i { display: none; }
        .pin-style::placeholder { color: #555; letter-spacing: 3px; font-size: 14px; font-family: 'Outfit', sans-serif; font-weight: 400; }

        .mn-btn { width: 100%; padding: 14px; border-radius: 12px; font-weight: 800; font-size: 13px; cursor: pointer; transition: all 0.3s ease; display: flex; justify-content: center; align-items: center; gap: 8px; border: none; letter-spacing: 1px; text-transform: uppercase; flex-shrink: 0; box-sizing: border-box; }
        .mn-btn:active { transform: scale(0.96); }
        .btn-gold { background: linear-gradient(135deg, var(--gold) 0%, #FFD700 100%); color: #000; box-shadow: 0 5px 25px var(--gold-glow); }
        .btn-green { background: linear-gradient(135deg, #25D366 0%, #128C7E 100%); color: #fff; box-shadow: 0 5px 20px rgba(37, 211, 102, 0.3); }
        .btn-dark { background: rgba(0,0,0,0.5); border: 1px solid var(--gold); color: var(--gold); backdrop-filter: blur(5px); }
        .btn-danger { background: rgba(255,51,51,0.1); border: 1px solid var(--danger); color: var(--danger); }

        .dash-header { width: 100%; max-width: 600px; display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; border-bottom: 1px dashed rgba(212,175,55,0.3); padding-bottom: 12px; flex-shrink: 0; overflow: hidden; }
        .status-badge { background: rgba(0, 250, 154, 0.1); border: 1px solid var(--neon-green); color: var(--neon-green); padding: 5px 10px; border-radius: 30px; font-size: 10px; font-weight: 800; display: flex; align-items: center; gap: 4px; letter-spacing: 1px; box-shadow: 0 0 15px rgba(0, 250, 154, 0.2); white-space: nowrap; }
        .status-badge.offline { background: rgba(255, 51, 51, 0.1); border-color: var(--danger); color: var(--danger); box-shadow: 0 0 15px rgba(255, 51, 51, 0.2); }
        
        .card { width: 100%; max-width: 600px; background: rgba(10,10,15,0.85); border: 1px solid rgba(255,255,255,0.05); padding: 15px; border-radius: 14px; margin-bottom: 15px; backdrop-filter: blur(15px); box-shadow: 0 10px 30px rgba(0,0,0,0.5); flex-shrink: 0; box-sizing: border-box; overflow: hidden; transition: opacity 0.3s ease; }
        .card.flex-grow { flex-grow: 1; max-height: none; }
        .card h3 { color: var(--gold); font-family: 'Cinzel'; margin-bottom: 12px; font-size: 15px; border-bottom: 1px dashed rgba(212,175,55,0.3); padding-bottom: 8px; display: flex; align-items: center; gap: 8px; }

        /* =========================================================================
           WHATSAPP-STYLE CHAT ENGINE - NATIVE DIRECT FULLSCREEN
           ========================================================================= */
        .chat-card { 
            padding: 0 !important; overflow: hidden; display: flex; flex-direction: column; 
            border: 1px solid rgba(255,255,255,0.1); border-radius: 16px; background: var(--wa-bg) !important; 
            margin-bottom: 15px; width: 100%; max-width: 600px; flex-shrink:0; 
            box-shadow: 0 20px 50px rgba(0,0,0,0.8); transition: all 0.4s cubic-bezier(0.2, 0.8, 0.2, 1); 
        }

        /* Fixed Chat Size - Increased Perfectly for Great UX */
        .chat-minimized { height: 65vh !important; min-height: 500px !important; max-height: 700px !important; }
        
        /* This applies ONLY when user manually clicks expand button */
        .chat-fullscreen { 
            position: fixed !important; inset: 0 !important; top: 0 !important; bottom: 0 !important; 
            left: 0 !important; right: 0 !important; height: 100% !important; min-height: 100dvh !important; 
            max-width: 100vw !important; width: 100vw !important; z-index: 9999999 !important; 
            border-radius: 0 !important; margin: 0 !important; border: none !important;
            background: var(--wa-bg) !important; padding-bottom: env(safe-area-inset-bottom) !important;
        }

        .chat-header { background: var(--wa-header); padding: 10px 15px; display: flex; justify-content: space-between; align-items: center; z-index: 2; box-shadow: 0 2px 10px rgba(0,0,0,0.5); flex-shrink: 0; }
        .chat-area { flex-grow: 1; overflow-y: auto; overflow-x: hidden; padding: 15px 12px; display: flex; flex-direction: column; gap: 10px; background-color: var(--wa-bg); background-image: radial-gradient(rgba(255,255,255,0.04) 1px, transparent 1px); background-size: 20px 20px; scroll-behavior: smooth; overscroll-behavior-y: none; }
        
        .bubble-wrapper { display: flex; align-items: center; width: 100%; position: relative; transition: background 0.2s; }
        .reply-swipe-icon { position: absolute; left: 10px; font-size: 18px; color: var(--wa-text); background: rgba(0,0,0,0.5); border-radius: 50%; width: 30px; height: 30px; display: flex; justify-content: center; align-items: center; opacity: 0; transform: scale(0.5); transition: all 0.2s cubic-bezier(0.2, 0.8, 0.2, 1); z-index: 1; }
        
        .chat-bubble { max-width: 85%; padding: 6px 8px 6px 12px; border-radius: 12px; position: relative; font-size: 14px; line-height: 1.4; color: var(--wa-text); display: inline-block; word-wrap: break-word; box-shadow: 0 1px 2px rgba(0,0,0,0.3); font-family: 'Outfit', sans-serif; font-weight: 400; z-index: 2; transition: transform 0.2s cubic-bezier(0.2, 0.8, 0.2, 1); }
        .chat-bubble:active { filter: brightness(1.1); }
        
        .chat-bubble.sent { align-self: flex-end; background: var(--wa-sent); border-top-right-radius: 0; margin-left: auto; } 
        .chat-bubble.received { align-self: flex-start; background: var(--wa-received); border-top-left-radius: 0; margin-right: auto; } 
        
        .chat-time { display: flex; align-items: center; gap: 4px; font-size: 10px; color: var(--wa-time); float: right; margin: 6px -2px -2px 10px; font-family: 'Outfit'; }
        .msg-status { font-size: 11px; color: #8696a0; }
        .msg-status.read { color: #53bdeb; } 
        
        .chat-reply-context { background: rgba(0,0,0,0.25); border-left: 4px solid var(--neon-green); padding: 5px 8px; border-radius: 6px; font-size: 12px; color: #ccc; margin-bottom: 5px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .chat-reply-context .sender { color: var(--neon-green); font-weight: 700; margin-bottom: 2px; font-size: 11px; }
        .received .chat-reply-context { border-left-color: var(--neon-cyan); }
        .received .chat-reply-context .sender { color: var(--neon-cyan); }
        
        .reply-banner { display: none; background: #202c33; padding: 8px 12px; align-items: center; border-bottom: 1px solid rgba(255,255,255,0.05); z-index: 5; flex-shrink: 0; }
        .reply-banner-content { flex-grow: 1; border-left: 4px solid var(--neon-green); padding-left: 10px; background: rgba(0,0,0,0.2); border-radius: 4px 8px 8px 4px; padding-top: 5px; padding-bottom: 5px; }
        .reply-banner-content .rep-title { font-size: 11px; color: var(--neon-green); font-weight: bold; margin-bottom: 2px; }
        .replying-to-text { color: #ccc; font-size: 12px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 90%; }
        .cancel-reply-btn { background: transparent; border: none; color: #8696a0; font-size: 18px; cursor: pointer; padding: 5px; }

        .chat-input-container { position: relative; padding: 8px 10px; background: var(--wa-header); z-index: 6; border-top: 1px solid rgba(255,255,255,0.03); width: 100%; box-sizing: border-box; flex-shrink: 0; }
        
        .chat-modern-pill { display: flex; align-items: center; background: var(--input-bg); border-radius: 30px; padding: 5px 6px; box-shadow: 0 2px 10px rgba(0,0,0,0.2); width: 100%; transition: all 0.3s ease; box-sizing: border-box; }
        
        .modern-left-btn { width: 34px; height: 34px; min-width: 34px; border-radius: 50%; background: var(--brand-purple); color: #fff; border: none; display: flex; justify-content: center; align-items: center; font-size: 14px; cursor: pointer; flex-shrink: 0; transition: transform 0.2s ease, box-shadow 0.2s; box-shadow: 0 2px 8px rgba(124, 58, 237, 0.4); margin-right: 8px; }
        .modern-left-btn:active { transform: scale(0.9); }
        
        .modern-input { flex-grow: 1; background: transparent; border: none; color: #fff; font-size: 14px; padding: 5px 0; outline: none; font-family: 'Outfit', sans-serif; min-width: 0; font-weight: 400; letter-spacing: 0.2px; }
        .modern-input::placeholder { color: var(--icon-gray); font-weight: 300; }
        
        .modern-right-icons { display: flex; align-items: center; gap: 12px; padding: 0 8px; color: var(--icon-gray); font-size: 18px; flex-shrink: 0; transition: opacity 0.3s ease, width 0.3s ease, transform 0.3s ease; overflow: hidden; }
        .modern-right-icons.hidden { opacity: 0; width: 0; padding: 0; transform: scale(0.8); pointer-events: none; }
        .modern-right-icons i { cursor: pointer; transition: 0.2s ease; }
        .modern-right-icons i:hover { color: #fff; transform: scale(1.1); }
        .modern-right-icons i:active { transform: scale(0.9); }

        .modern-send-btn { width: 0; height: 34px; border-radius: 20px; background: var(--brand-purple); color: #fff; border: none; display: flex; justify-content: center; align-items: center; font-size: 14px; cursor: pointer; transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1); opacity: 0; overflow: hidden; box-shadow: 0 2px 8px rgba(124, 58, 237, 0.4); }
        .modern-send-btn.active { width: 45px; opacity: 1; margin-left: 6px; }
        .modern-send-btn:active { transform: scale(0.9); }
        .modern-send-btn i { transform: translateX(-1px); }

        .recording-ui { display: none; align-items: center; justify-content: space-between; width: 100%; background: #2a3942; border-radius: 30px; padding: 5px 6px; box-shadow: inset 0 2px 5px rgba(0,0,0,0.2); }
        .blinking-dot { width: 10px; height: 10px; background: var(--danger); border-radius: 50%; display: inline-block; animation: blink 1s infinite; margin-right: 6px; }
        @keyframes blink { 0% { opacity: 1; } 50% { opacity: 0.3; } 100% { opacity: 1; } }
        .rec-timer { color: var(--danger); font-family: 'Orbitron', monospace; font-size: 13px; font-weight: bold; flex-grow: 1; padding-left: 4px; }

        .sticker-drawer { position: absolute; bottom: 60px; left: 10px; right: 10px; background: #1f2937; border-radius: 14px; padding: 12px; box-shadow: 0 15px 40px rgba(0,0,0,0.6); opacity: 0; transform: translateY(20px) scale(0.95); pointer-events: none; transition: all 0.3s cubic-bezier(0.2, 0.8, 0.2, 1); z-index: 10; border: 1px solid rgba(255,255,255,0.05); }
        .sticker-drawer.show { opacity: 1; transform: translateY(0) scale(1); pointer-events: auto; }
        .sticker-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; max-height: 180px; overflow-y: auto; padding: 5px; }
        .sticker-grid img { width: 100%; height: auto; cursor: pointer; transition: 0.2s; border-radius: 8px; filter: drop-shadow(0 2px 5px rgba(0,0,0,0.3)); }
        .sticker-grid img:hover { transform: scale(1.15); }

        .attachment-drawer { position: absolute; bottom: 60px; left: 10px; right: 10px; background: #1f2937; border-radius: 14px; padding: 15px; box-shadow: 0 15px 40px rgba(0,0,0,0.6); display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; opacity: 0; transform: translateY(20px) scale(0.95); pointer-events: none; transition: all 0.3s cubic-bezier(0.2, 0.8, 0.2, 1); z-index: 10; border: 1px solid rgba(255,255,255,0.05); }
        .attachment-drawer.show { opacity: 1; transform: translateY(0) scale(1); pointer-events: auto; }
        .attach-item { display: flex; flex-direction: column; align-items: center; gap: 6px; cursor: pointer; transition: 0.2s; }
        .attach-item:hover { transform: translateY(-3px); }
        .attach-icon-circle { width: 45px; height: 45px; border-radius: 50%; display: flex; justify-content: center; align-items: center; font-size: 18px; color: #fff; box-shadow: 0 5px 15px rgba(0,0,0,0.3); }
        .attach-item span { font-size: 10px; color: #d1d5db; font-family: 'Outfit'; font-weight: 500; }
        
        .bg-doc { background: #6366f1; } .bg-cam { background: #ec4899; } .bg-gal { background: #a855f7; }
        .bg-aud { background: #f97316; } .bg-loc { background: #10b981; } .bg-con { background: #3b82f6; }

        /* =========================================================================
           LONG PRESS MODAL OVERLAYS (NATIVE CONTEXT MENUS)
           ========================================================================= */
        .long-press-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.6); z-index: 99999; backdrop-filter: blur(2px); justify-content: center; align-items: center; }
        .long-press-modal { background: #233138; border-radius: 12px; width: 85%; max-width: 320px; overflow: hidden; box-shadow: 0 15px 50px rgba(0,0,0,0.5); transform: scale(0.9); opacity: 0; transition: all 0.2s cubic-bezier(0.2, 0.8, 0.2, 1); }
        .long-press-overlay.active { display: flex; }
        .long-press-overlay.active .long-press-modal { transform: scale(1); opacity: 1; }
        
        .msg-info-pane { padding: 12px 15px; background: rgba(0,0,0,0.2); border-bottom: 1px solid rgba(255,255,255,0.05); }
        .msg-info-row { display: flex; justify-content: space-between; font-size: 12px; color: #8696a0; margin-bottom: 4px; }
        .msg-info-row:last-child { margin-bottom: 0; }
        .msg-info-row span.val { color: #e9edef; font-weight: 500; }
        
        .modal-action-btn { width: 100%; padding: 12px 15px; text-align: left; background: transparent; border: none; border-bottom: 1px solid rgba(255,255,255,0.05); color: #e9edef; font-size: 14px; cursor: pointer; display: flex; align-items: center; gap: 12px; transition: 0.2s; font-family: 'Outfit'; }
        .modal-action-btn:last-child { border-bottom: none; }
        .modal-action-btn:active { background: rgba(255,255,255,0.05); }
        .modal-action-btn i { font-size: 16px; width: 22px; text-align: center; }
        .modal-action-btn.delete { color: #ef4444; }

        /* =========================================================================
           CALLING OVERLAYS (WEBRTC) + INSTAGRAM STYLE NOTIFICATION
           ========================================================================= */
        #incoming-call-overlay { display: none; }
        #incoming-call-overlay.active { display: flex; }
        
        @keyframes glowPulse {
            0% { box-shadow: 0 0 20px var(--brand-purple), 0 0 40px var(--brand-purple); transform: scale(1); }
            100% { box-shadow: 0 0 50px var(--brand-purple), 0 0 80px var(--neon-cyan); transform: scale(1.05); }
        }

        #active-call-overlay { display: none; position: fixed; inset: 0; background: #000; z-index: 999999999; flex-direction: column; overflow: hidden; }
        
        /* =========================================================================
           TIMELINE, HISTORY & MAP (RESPONSIVE FIXES)
           ========================================================================= */
        .client-list-item { background: rgba(0,0,0,0.6); padding: 12px; border-radius: 10px; border-left: 4px solid var(--neon-cyan); margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; transition: 0.3s; cursor: pointer; box-shadow: 0 5px 15px rgba(0,0,0,0.3); }
        .client-list-item:hover { background: rgba(212, 175, 55, 0.1); border-left-color: var(--gold); transform: translateY(-2px); }
        .client-list-item h4 { margin:0 0 3px 0; font-size: 14px; color: #fff; font-family: 'Cinzel', serif; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .client-list-item p { margin:0; font-size: 11px; color: #888; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .client-list-actions { display: flex; align-items: center; gap: 8px; }
        .client-list-pin { font-family: 'Orbitron', monospace; font-size: 14px; color: var(--gold); font-weight: bold; background: rgba(212,175,55,0.1); padding: 5px 8px; border-radius: 6px; letter-spacing: 1px; }
        
        .action-icon-btn { background: transparent; border: none; color: #888; font-size: 16px; cursor: pointer; transition: 0.3s; padding: 5px; }
        .action-icon-btn:hover { color: var(--neon-cyan); transform: scale(1.1); }
        .action-icon-btn.trash { color: rgba(255,51,51,0.7); }
        .action-icon-btn.trash:hover { color: var(--danger); transform: scale(1.2); }

        .map-wrapper { width: 100%; height: 220px; border-radius: 10px; overflow: hidden; border: 2px solid #222; position: relative; background: #050505; margin-bottom: 12px; }
        .map-iframe { width: 100%; height: 100%; border: none; filter: invert(100%) hue-rotate(180deg) brightness(85%) contrast(110%) sepia(30%); pointer-events: auto; transition: 1s ease; opacity: 0; }
        .map-iframe.loaded { opacity: 1; }
        .map-loader { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: var(--gold); font-size: 11px; text-align: center; font-weight: bold; letter-spacing: 1px; z-index: 1; }
        
        .location-display { background: rgba(0,229,255,0.05); border: 1px solid rgba(0,229,255,0.2); border-radius: 8px; padding: 10px; margin-bottom: 12px; display: flex; align-items: center; gap: 10px; }
        .location-icon { width: 30px; height: 30px; border-radius: 50%; background: rgba(0,229,255,0.15); display: flex; justify-content: center; align-items: center; color: var(--neon-cyan); font-size: 14px; flex-shrink: 0; }
        .location-text h4 { margin: 0 0 2px 0; font-size: 9px; color: #888; text-transform: uppercase; letter-spacing: 1px; }
        .location-text p { margin: 0; font-size: 13px; color: #fff; font-weight: 700; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 100%; }

        .telemetrics-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 6px; margin-bottom: 10px; }
        .metric-box { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.1); border-radius: 6px; padding: 8px; text-align: center; }
        .metric-label { font-size: 9px; color: #888; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 3px; }
        .metric-value { font-family: 'Orbitron', sans-serif; font-size: 12px; font-weight: 700; color: var(--neon-cyan); }

        .timeline-feed { padding-left: 15px; position: relative; flex-grow: 1; overflow-y: auto; padding-right: 5px; min-height: 180px; margin-bottom: 10px; }
        .timeline-item { position: relative; margin-bottom: 15px; padding-bottom: 15px; border-bottom: 1px dashed rgba(255,255,255,0.05); animation: slideInLeft 0.4s ease forwards; display: flex; justify-content: space-between; align-items: flex-start; }
        .timeline-item:last-child { border-bottom: none; margin-bottom: 0; padding-bottom: 0; }
        .timeline-item::before { content: ''; position: absolute; left: -21px; top: 3px; width: 10px; height: 10px; border-radius: 50%; background: var(--neon-green); box-shadow: 0 0 10px var(--neon-green); border: 2px solid #111; z-index: 2; }
        .timeline-item::after { content: ''; position: absolute; left: -17px; top: 15px; width: 2px; height: 100%; background: rgba(0, 250, 154, 0.3); z-index: 1; }
        .timeline-item:last-child::after { display: none; }
        
        .update-content { width: 85%; }
        .update-time { font-size: 9px; color: var(--gold); margin-bottom: 4px; font-family: 'Orbitron', sans-serif; font-weight: bold; text-transform: uppercase; letter-spacing: 1px; }
        .update-text { font-size: 13px; color: #eee; line-height: 1.4; font-weight: 500; word-wrap: break-word; }

        .quick-actions { display: flex; gap: 5px; margin-bottom: 12px; flex-wrap: wrap; }
        .quick-btn { background: rgba(255,255,255,0.05); border: 1px solid rgba(212,175,55,0.4); color: #fff; padding: 6px 8px; border-radius: 6px; font-size: 10px; cursor: pointer; transition: 0.3s; font-family: 'Orbitron', sans-serif; flex-grow: 1; text-align: center; }
        .quick-btn:hover { background: rgba(212,175,55,0.2); border-color: var(--gold); color: var(--gold); }
        
        .target-lock-banner { background: rgba(0, 229, 255, 0.1); border: 1px dashed var(--neon-cyan); padding: 10px 12px; border-radius: 8px; margin-bottom: 15px; display: flex; align-items: center; gap: 12px; flex-shrink: 0; width: 100%; max-width: 600px; box-sizing: border-box; }
        .target-lock-banner i { color: var(--neon-cyan); font-size: 20px; animation: targetPulse 2s infinite; }
        .target-lock-details { overflow: hidden; }
        .target-lock-details h4 { margin: 0; color: var(--neon-cyan); font-size: 12px; font-family: 'Orbitron'; letter-spacing: 1px; text-transform: uppercase; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .target-lock-details p { margin: 2px 0 0 0; color: #fff; font-size: 11px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        @keyframes targetPulse { 0%, 100% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.1); opacity: 0.5; } }

        /* =========================================================================
           PREMIUM MEDIA VIEWER (Fullscreen Images & PDFs)
           ========================================================================= */
        #premium-media-viewer {
            display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.98); backdrop-filter: blur(15px);
            z-index: 9999999999; flex-direction: column; animation: fadeInView 0.3s ease forwards;
        }
        .media-viewer-header {
            display: flex; justify-content: space-between; align-items: center; padding: 15px 20px;
            background: linear-gradient(to bottom, rgba(0,0,0,0.8), transparent); flex-shrink: 0;
        }
        .media-viewer-content {
            flex-grow: 1; display: flex; justify-content: center; align-items: center; overflow: hidden; padding: 10px;
        }
        .media-viewer-content img {
            max-width: 100%; max-height: 100%; object-fit: contain; border-radius: 12px; box-shadow: 0 15px 50px rgba(0,0,0,0.8);
            transition: transform 0.3s ease;
        }
        .media-viewer-content iframe, .media-viewer-content embed, .media-viewer-content object {
            width: 100%; height: 100%; border: none; border-radius: 12px; background: #fff;
        }

        /* Settings Toggle UI */
        .switch { position: relative; display: inline-block; width: 42px; height: 22px; margin-left: 10px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; inset: 0; background-color: rgba(255,255,255,0.2); transition: .4s; border-radius: 22px; }
        .slider:before { position: absolute; content: ""; height: 16px; width: 16px; left: 3px; bottom: 3px; background-color: white; transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--neon-green); box-shadow: 0 0 10px rgba(0,250,154,0.4); }
        input:checked + .slider:before { transform: translateX(20px); }
    </style>
</head>
<body> 

    <div class="bg-map"></div>
    <div class="grid-overlay"></div>
    <div id="toast-container"></div>

    <!-- Notification History Drawer -->
    <div id="notification-drawer" class="long-press-overlay" onclick="closeNotifDrawer()">
        <div class="long-press-modal" style="height: 60vh; max-height: 400px; display: flex; flex-direction: column; padding: 0; max-width: 350px; background: rgba(15,15,20,0.98); border: 1px solid rgba(212,175,55,0.3);" onclick="event.stopPropagation()">
            <div style="padding: 15px; background: rgba(0,0,0,0.8); border-bottom: 2px solid var(--gold); display: flex; justify-content: space-between; align-items: center;">
                <h3 style="color: var(--gold); font-family: 'Cinzel'; margin: 0; font-size: 16px;"><i class="fas fa-bell"></i> System Alerts</h3>
                <i class="fas fa-times" style="color: #fff; cursor: pointer; font-size: 18px;" onclick="closeNotifDrawer()"></i>
            </div>
            <div id="notif-list-area" style="flex-grow: 1; overflow-y: auto; padding: 15px; background: transparent; display: flex; flex-direction: column; gap: 10px;">
                <div style="text-align:center; color:#555; font-size:12px; padding:20px;">No recent alerts.</div>
            </div>
        </div>
    </div>

    <!-- Settings Overlay Modal -->
    <div id="settings-overlay" class="long-press-overlay" onclick="closeSettings()">
        <div class="long-press-modal" style="padding: 20px; display: flex; flex-direction: column; gap: 15px;" onclick="event.stopPropagation()">
            <div style="display:flex; justify-content:space-between; align-items:center; border-bottom:1px solid rgba(255,255,255,0.1); padding-bottom:10px;">
                <h3 style="margin:0; font-family:'Cinzel'; color:var(--gold); font-size:16px;"><i class="fas fa-cog"></i> Settings</h3>
                <i class="fas fa-times" style="color:#fff; cursor:pointer; font-size:18px;" onclick="closeSettings()"></i>
            </div>
            
            <button class="modal-action-btn" onclick="goHome()"><i class="fas fa-home" style="color: var(--neon-cyan);"></i> Dashboard Home</button>
            
            <div style="display:flex; justify-content:space-between; align-items:center; padding: 12px 15px; border-bottom: 1px solid rgba(255,255,255,0.05); background:rgba(0,0,0,0.2); border-radius:8px;">
                <span style="font-size:14px; font-weight:500; color:#fff;"><i class="fas fa-volume-up" style="color:var(--neon-green); width:22px;"></i> App Sounds</span>
                <label class="switch">
                    <input type="checkbox" id="toggle-sound" onchange="toggleSetting('sound', this.checked)">
                    <span class="slider round"></span>
                </label>
            </div>
            
            <div style="display:flex; justify-content:space-between; align-items:center; padding: 12px 15px; background:rgba(0,0,0,0.2); border-radius:8px;">
                <span style="font-size:14px; font-weight:500; color:#fff;"><i class="fas fa-mobile-alt" style="color:var(--brand-purple); width:22px;"></i> Vibration Alerts</span>
                <label class="switch">
                    <input type="checkbox" id="toggle-vibration" onchange="toggleSetting('vibration', this.checked)">
                    <span class="slider round"></span>
                </label>
            </div>
        </div>
    </div>

    <!-- Premium Media Viewer Overlay -->
    <div id="premium-media-viewer">
        <div class="media-viewer-header">
            <div id="media-title" style="color:var(--gold); font-family:'Cinzel'; font-weight:bold; font-size:16px;">Document Viewer</div>
            <i class="fas fa-times" style="color:#fff; font-size:24px; cursor:pointer; text-shadow:0 2px 10px rgba(0,0,0,0.5);" onclick="closePremiumMediaViewer()"></i>
        </div>
        <div id="media-content-container" class="media-viewer-content">
            <!-- Media will be injected here dynamically -->
        </div>
        <div style="padding:15px; text-align:center; background: linear-gradient(to top, rgba(0,0,0,0.8), transparent); flex-shrink:0;">
            <a id="media-download-btn" href="#" download class="mn-btn btn-gold" style="display:inline-flex; width:auto; padding:12px 25px; box-shadow:0 10px 30px rgba(212,175,55,0.4);"><i class="fas fa-download"></i> SAVE TO DEVICE</a>
        </div>
    </div>

    <div id="install-banner" class="install-banner">
        <div style="display:flex; align-items:center; gap:12px;">
            <img src="https://i.postimg.cc/52vLtJBM/1000095487-(2).png" style="width:35px; height:35px; border-radius:8px; box-shadow: 0 4px 10px rgba(0,0,0,0.5);">
            <div>
                <h4 style="margin:0; color:var(--gold); font-size:13px; font-family:'Cinzel';">MND Tracking App</h4>
                <p style="margin:0; color:#aaa; font-size:10px;">Install for full-screen experience</p>
            </div>
        </div>
        <div style="display:flex; align-items:center; gap:8px;">
            <button class="mn-btn btn-gold" style="padding:6px 12px; width:auto; font-size:11px; margin:0; border-radius: 6px;" onclick="installApp()">INSTALL</button>
            <button style="background:transparent; border:none; color:#888; font-size:18px; padding:5px; cursor:pointer;" onclick="closeInstallBanner()"><i class="fas fa-times"></i></button>
        </div>
    </div>

    <div id="msg-action-overlay" class="long-press-overlay" onclick="closeMsgModal()">
        <div class="long-press-modal" onclick="event.stopPropagation()">
            <div class="msg-info-pane">
                <div class="msg-info-row"><span>Sent:</span> <span class="val" id="modal-time-sent">--</span></div>
                <div class="msg-info-row"><span>Status:</span> <span class="val" id="modal-time-read">--</span></div>
            </div>
            <button class="modal-action-btn" onclick="execModalReply()"><i class="fas fa-reply"></i> Reply</button>
            <button class="modal-action-btn" onclick="execModalCopy()"><i class="fas fa-copy"></i> Copy Text</button>
            <button class="modal-action-btn" id="modal-btn-edit" onclick="execModalEdit()" style="color: var(--neon-cyan);"><i class="fas fa-pen"></i> Edit Message</button>
            <button class="modal-action-btn delete" id="modal-btn-delete" onclick="execModalDelete()"><i class="fas fa-trash"></i> Delete Message</button>
        </div>
    </div>

    <!-- Instagram-Style Incoming Call Notification UI -->
    <div id="incoming-call-overlay" class="long-press-overlay" style="background: rgba(0,0,0,0.85); backdrop-filter: blur(25px); flex-direction: column; justify-content: space-evenly; align-items: center; padding: 40px 20px; z-index: 9999999999;">
        <div style="text-align: center; margin-top: 15vh;">
            <div class="attach-icon-circle bg-cam" style="margin: 0 auto 25px auto; width: 120px; height: 120px; font-size: 50px; animation: glowPulse 2s infinite ease-in-out; background: var(--brand-purple);">
                <i class="fas fa-video"></i>
            </div>
            <h2 id="incoming-call-name" style="color: #fff; font-size: 32px; font-weight: 800; margin-bottom: 10px; font-family: 'Outfit';">HQ Dispatch</h2>
            <p style="color: var(--neon-cyan); font-size: 16px; font-family: 'Outfit'; font-weight: 500; letter-spacing: 1px; animation: blink 1.5s infinite;">MND Secure Video Call...</p>
        </div>
        
        <div style="display: flex; justify-content: space-around; width: 100%; max-width: 350px; margin-bottom: 10vh;">
            <div style="display: flex; flex-direction: column; align-items: center; gap: 12px;">
                <button class="mn-btn btn-danger" style="width: 75px; height: 75px; border-radius: 50%; padding: 0; margin: 0; font-size: 30px; box-shadow: 0 10px 30px rgba(255,51,51,0.4);" onclick="declineCall()"><i class="fas fa-phone-slash"></i></button>
                <span style="color: #fff; font-size: 15px; font-weight: 600;">Decline</span>
            </div>
            <div style="display: flex; flex-direction: column; align-items: center; gap: 12px;">
                <button class="mn-btn btn-green" style="width: 75px; height: 75px; border-radius: 50%; padding: 0; margin: 0; background: #25D366; font-size: 30px; box-shadow: 0 10px 30px rgba(37,211,102,0.4); animation: targetPulse 1s infinite alternate;" onclick="acceptCall()"><i class="fas fa-phone"></i></button>
                <span style="color: #fff; font-size: 15px; font-weight: 600;">Accept</span>
            </div>
        </div>
    </div>

    <!-- ACTIVE CALL OVERLAY (Modified to guarantee Audio routing) -->
    <div id="active-call-overlay">
        <video id="remote-video" autoplay playsinline style="width:100%; height:100%; object-fit:cover; position:absolute; inset:0; z-index:1;"></video>
        <!-- Adding explicit audio tag to guarantee Mobile Voice playback -->
        <audio id="remote-audio" autoplay playsinline></audio>
        
        <video id="local-video" autoplay playsinline muted style="width:110px; height:150px; object-fit:cover; position:absolute; bottom:120px; right:20px; z-index:2; border-radius:12px; border:2px solid var(--gold); box-shadow:0 10px 20px rgba(0,0,0,0.8); background:#222;"></video>
        
        <div style="position:absolute; top:50px; left:0; right:0; text-align:center; z-index:3; text-shadow:0 2px 5px rgba(0,0,0,0.8);">
            <h2 id="active-call-name" style="color:#fff; margin:0; font-family:'Outfit'; font-weight:600; font-size:24px;">HQ Dispatch</h2>
            <p id="active-call-status" style="color:var(--neon-green); margin:5px 0 0 0; font-size:14px; font-family:'Orbitron';">Connecting...</p>
        </div>

        <div style="position:absolute; bottom:40px; left:0; right:0; display:flex; justify-content:center; align-items:center; gap:20px; z-index:3;">
            <button class="mn-btn" style="width:55px; height:55px; border-radius:50%; padding:0; background:rgba(255,255,255,0.2); backdrop-filter:blur(10px); color:#fff; border:1px solid rgba(255,255,255,0.3); font-size:20px;" onclick="toggleMic()" id="btn-toggle-mic"><i class="fas fa-microphone"></i></button>
            <button class="mn-btn btn-danger" style="width:70px; height:70px; border-radius:50%; padding:0; font-size:24px; box-shadow: 0 5px 20px rgba(255,51,51,0.5);" onclick="endCall()"><i class="fas fa-phone-slash"></i></button>
            <button class="mn-btn" style="width:55px; height:55px; border-radius:50%; padding:0; background:rgba(255,255,255,0.2); backdrop-filter:blur(10px); color:#fff; border:1px solid rgba(255,255,255,0.3); font-size:20px;" onclick="toggleCam()" id="btn-toggle-cam"><i class="fas fa-video"></i></button>
            <button class="mn-btn" style="width:45px; height:45px; border-radius:50%; padding:0; background:rgba(255,255,255,0.2); backdrop-filter:blur(10px); color:#fff; border:1px solid rgba(255,255,255,0.3); font-size:16px;" onclick="flipCamera()"><i class="fas fa-sync-alt"></i></button>
        </div>
    </div>

    <div id="dispatch-overlay" class="long-press-overlay" onclick="closeDispatchModal()">
        <div class="long-press-modal" style="padding: 20px; display: flex; flex-direction: column; gap: 15px;" onclick="event.stopPropagation()">
            <div style="display:flex; justify-content:space-between; align-items:center; border-bottom:1px solid rgba(255,255,255,0.1); padding-bottom:10px;">
                <h3 style="margin:0; font-family:'Cinzel'; color:var(--gold); font-size:16px;">Dispatch Links</h3>
                <div style="display:flex; gap:5px; background:rgba(0,0,0,0.5); border-radius:8px; padding:3px;">
                    <button id="lang-btn-en" style="background:var(--gold); color:#000; border:none; padding:4px 8px; border-radius:6px; font-weight:bold; font-size:10px; cursor:pointer; transition:0.3s;" onclick="toggleDispatchLang('en')">EN</button>
                    <button id="lang-btn-hi" style="background:transparent; color:#fff; border:none; padding:4px 8px; border-radius:6px; font-weight:bold; font-size:10px; opacity:0.5; cursor:pointer; transition:0.3s;" onclick="toggleDispatchLang('hi')">HI</button>
                </div>
            </div>
            
            <button class="modal-action-btn" style="color:#25D366; justify-content:center; background:rgba(37,211,102,0.1); border-radius:8px;" onclick="dispatchVia('whatsapp')"><i class="fab fa-whatsapp"></i> WhatsApp</button>
            <button class="modal-action-btn" style="color:#3b82f6; justify-content:center; background:rgba(59,130,246,0.1); border-radius:8px;" onclick="dispatchVia('sms')"><i class="fas fa-sms"></i> SMS Message</button>
            <button class="modal-action-btn" style="color:#e1306c; justify-content:center; background:rgba(225,48,108,0.1); border-radius:8px;" onclick="dispatchVia('instagram')"><i class="fab fa-instagram"></i> Instagram</button>
            <button class="modal-action-btn" style="color:#1877f2; justify-content:center; background:rgba(24,119,242,0.1); border-radius:8px;" onclick="dispatchVia('facebook')"><i class="fab fa-facebook"></i> Facebook</button>
        </div>
    </div>

    <input type="file" id="file-gallery" accept="image/*,video/*" style="display:none;" onchange="handleFileUpload(event, 'image')">
    <input type="file" id="file-doc" accept=".pdf,.doc,.docx,.txt,.xls,.xlsx" style="display:none;" onchange="handleFileUpload(event, 'document')">
    <input type="file" id="file-audio" accept="audio/*" style="display:none;" onchange="handleFileUpload(event, 'audio')">
    <input type="file" id="file-camera" accept="image/*" capture="environment" style="display:none;" onchange="handleFileUpload(event, 'camera')">

    <div id="view-gatekeeper" class="view-container active-view" style="justify-content: center;">
        <div class="gatekeeper-box" style="background: rgba(15,15,20,0.85); position:relative; overflow:hidden; border-radius:20px; padding:30px 20px; box-shadow: 0 20px 60px rgba(0,0,0,0.9); border:2px solid var(--gold); max-width:400px; width: 100%;">
            <h1 class="app-title"><i class="fas fa-satellite-dish"></i> MND TRACKING</h1>
            <p class="app-subtitle">Secured Cloud Logistics Portal.<br>Enter your Mobile Number and PIN below.</p>
            
            <div class="input-group">
                <i class="fas fa-phone-alt"></i>
                <input type="tel" id="login-phone" class="mn-input" placeholder="Mobile Number *" autocomplete="off">
            </div>
            
            <div class="input-group">
                <input type="password" id="login-pin" class="mn-input pin-style" placeholder="6 DIGIT PIN" maxlength="6" autocomplete="off">
            </div>

            <div style="display: flex; gap: 10px; margin-top: 15px;">
                <button class="mn-btn btn-dark" id="clear-btn" onclick="clearLogin(event)" style="flex: 0.5;"><i class="fas fa-eraser"></i> STOP & CLEAR</button>
                <button class="mn-btn btn-gold" id="login-btn" onclick="processLogin(event)" style="flex: 1.5;"><i class="fas fa-fingerprint"></i> INITIATE HANDSHAKE</button>
            </div>
            
            <p id="login-error" style="display:none; color:var(--danger); font-size:12px; font-weight:bold; margin-top:12px; text-align:center;"></p>
        </div>
        <div class="app-footer">Powered by Maa Nirmala DJ</div>
    </div>

    <div id="view-admin" class="view-container">
        <div class="dash-header">
            <div>
                <h2 style="color:var(--gold); font-family:'Cinzel'; margin:0; font-size:18px;">Command Center</h2>
                <span style="font-size:10px; color:#888;">Authorized Personnel Only</span>
            </div>
            <div style="display:flex; align-items:center; gap:12px;">
                <div class="status-badge"><i class="fas fa-link"></i> DB SYNCED</div>
                <i class="fas fa-bell" style="color:var(--gold); font-size:18px; cursor:pointer;" onclick="openNotifDrawer()"></i>
                <i class="fas fa-cog" style="color:#aaa; font-size:18px; cursor:pointer;" onclick="openSettings()"></i>
            </div>
        </div>

        <div class="card">
            <h3><i class="fas fa-key"></i> Provision / Update</h3>
            <p style="font-size:10px; color:#aaa; margin-bottom:12px;">Entering an existing number will elegantly UPDATE their PIN without deleting history.</p>
            
            <div style="display:flex; gap:8px; margin-bottom:10px; width: 100%;">
                <input type="text" id="admin-c-name" class="mn-input" style="padding:12px; flex:1;" placeholder="Client Name *">
                <input type="tel" id="admin-c-phone" class="mn-input" style="padding:12px; flex:1;" placeholder="Mobile No. *">
            </div>
            
            <div style="display:flex; gap:8px; margin-bottom:12px; width: 100%;">
                <input type="text" id="admin-c-event" class="mn-input" style="flex:1.5; padding:12px;" placeholder="Event Ref *">
                <input type="text" id="admin-c-custom-pin" class="mn-input" style="flex:1; padding:12px; font-family:'Orbitron'; text-align:center; letter-spacing:2px; color:var(--neon-cyan);" placeholder="Custom PIN" maxlength="6">
            </div>
            
            <button class="mn-btn btn-gold" id="btn-generate" onclick="adminGeneratePin(event)"><i class="fas fa-microchip"></i> AUTHORIZE & SAVE</button>

            <div id="admin-pin-result" style="display:none; margin-top:15px; text-align:center; background: rgba(0,0,0,0.6); padding: 15px; border-radius: 10px; border: 1px dashed var(--neon-green);">
                <p style="color:#aaa; font-size:10px; text-transform:uppercase; letter-spacing:1px;">Client Access Ready:</p>
                <div style="font-family:'Orbitron', monospace; font-size:32px; color:var(--neon-green); font-weight:900; letter-spacing:6px; margin:8px 0; text-shadow: 0 0 15px rgba(0,250,154,0.4);" id="display-new-pin">000000</div>
                <button class="mn-btn btn-green" onclick="openDispatchModal(event)"><i class="fas fa-share-alt"></i> DISPATCH OPTIONS</button>
            </div>
        </div>

        <div class="card" style="border-color:var(--danger); background:rgba(255,51,51,0.05);">
            <h3 style="color:var(--danger); border-bottom-color:rgba(255,51,51,0.3);"><i class="fas fa-broadcast-tower"></i> Global Broadcast System</h3>
            <p style="font-size:10px; color:#aaa; margin-bottom:12px;">Send an instant push notification alert to EVERY single client in the database.</p>
            <textarea id="admin-global-msg" class="mn-input" rows="2" style="resize:none; padding:12px; border-color:rgba(255,51,51,0.3);" placeholder="Message all active clients..."></textarea>
            <button class="mn-btn btn-danger" style="margin-top:10px; background:linear-gradient(135deg, #ff3333 0%, #cc0000 100%);" onclick="sendGlobalAlert()"><i class="fas fa-bullhorn"></i> SEND GLOBAL ALERT</button>
            
            <!-- Admin Global Broadcast History -->
            <div style="border-top: 1px dashed rgba(255,51,51,0.3); padding-top:12px; margin-top: 15px;">
                <h4 style="font-size:10px; color:#aaa; margin-bottom:8px; text-transform:uppercase;">Recent Broadcasts (Admin Only):</h4>
                <div id="admin-global-history-list" class="timeline-feed" style="max-height: 150px; overflow-y: auto;">
                    <div style="text-align:center; color:#555; font-size:12px; padding:10px;">No global alerts.</div>
                </div>
            </div>
        </div>

        <div class="card flex-grow">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom: 5px; flex-shrink: 0;">
                <h3 style="margin:0; border:none; padding:0;"><i class="fas fa-history"></i> Live Network</h3>
                <span style="font-size:10px; background:rgba(212,175,55,0.1); padding:3px 6px; border-radius:4px; color:var(--gold);">Auto-Sync</span>
            </div>
            <p style="font-size:10px; color:#888; margin-bottom:12px; flex-shrink:0;">Click client below to open Targeted Live Session.</p>
            
            <div id="admin-active-list" style="flex-grow:1; overflow-y:auto; padding-right: 5px;">
                <div style="text-align:center; color:#555; font-size:12px; padding:20px;"><i class="fas fa-circle-notch fa-spin"></i> Fetching history logs...</div>
            </div>
        </div>

        <div style="width: 100%; max-width: 600px; display: flex; flex-direction: column; gap: 10px; margin-top: 10px; flex-shrink: 0;">
            <button class="mn-btn btn-danger" onclick="systemLogout(event)">
                <i class="fas fa-power-off"></i> TERMINATE SYSTEM
            </button>
            <a href="https://maa-nirmala-dj.github.io/-tent-house./" target="_blank" class="mn-btn btn-dark" style="text-decoration: none;">
                <i class="fas fa-globe"></i> VISIT OFFICIAL WEBSITE
            </a>
            <a href="https://maa-nirmala-dj.github.io/WELCOME-TO-MND-HUB/" target="_blank" class="mn-btn btn-gold" style="text-decoration: none; background: linear-gradient(135deg, var(--brand-purple) 0%, #9333ea 100%); color:#fff; box-shadow: 0 5px 20px rgba(124,58,237,0.4); border:none;">
                <i class="fas fa-layer-group"></i> MND HUB PORTAL
            </a>
        </div>
        <div class="app-footer">Powered by Maa Nirmala DJ</div>
    </div>

    <div id="view-admin-session" class="view-container">
        <div class="dash-header" style="border-color:var(--neon-cyan);">
            <div style="overflow: hidden;">
                <h2 style="color:var(--neon-cyan); font-family:'Orbitron'; margin:0; font-size:16px;">Targeted Session</h2>
                <span style="font-size:10px; color:#888;">Direct Encrypted Link</span>
            </div>
            <div style="display:flex; align-items:center; gap:12px;">
                <i class="fas fa-cog" style="color:#aaa; font-size:18px; cursor:pointer;" onclick="openSettings()"></i>
                <i class="fas fa-bell" style="color:var(--gold); font-size:18px; cursor:pointer;" onclick="openNotifDrawer()"></i>
                <button class="action-icon-btn" onclick="closeAdminSession(event)" style="color:var(--gold); border:1px solid var(--gold); border-radius:50%; width:30px; height:30px; background:rgba(212,175,55,0.1); flex-shrink: 0;"><i class="fas fa-times"></i></button>
            </div>
        </div>

        <div class="target-lock-banner">
            <i class="fas fa-crosshairs"></i>
            <div class="target-lock-details">
                <h4>Active Target Locked</h4>
                <p><span id="session-c-name" style="font-weight:bold;">Name</span> | <span id="session-c-event">Event</span> | <span id="session-c-phone" style="color:var(--gold);">Phone</span></p>
            </div>
        </div>

        <div class="card" id="admin-client-loc-card" style="display:none; border-color:var(--neon-green); background:rgba(0,250,154,0.05);">
            <h3 style="color:var(--neon-green); border-bottom-color:rgba(0,250,154,0.3);"><i class="fas fa-street-view"></i> Target Event Location</h3>
            <p style="font-size:11px; color:#aaa; margin-bottom:12px;">The client has shared their exact event coordinates.</p>
            <a id="admin-view-client-map" href="#" target="_blank" class="mn-btn btn-green" style="font-size:12px; padding:10px; text-decoration:none;"><i class="fas fa-map-marker-alt"></i> OPEN IN MAPS</a>
        </div>

        <div class="card">
            <h3 style="color:var(--neon-cyan); border-bottom-color:rgba(0, 229, 255, 0.3);"><i class="fas fa-satellite"></i> Direct GPS Uplink</h3>
            <div class="telemetrics-grid" id="admin-telemetrics" style="display:none;">
                <div class="metric-box"><div class="metric-label">Speed</div><div class="metric-value" id="a-speed">0 km/h</div></div>
                <div class="metric-box"><div class="metric-label">Accuracy</div><div class="metric-value" id="a-acc">0 m</div></div>
                <div class="metric-box"><div class="metric-label">Pings</div><div class="metric-value" id="a-ping">0</div></div>
            </div>
            
            <div id="admin-ai-location" class="location-display" style="display:none; border-color:var(--neon-cyan); background:rgba(0,229,255,0.05);">
                <div class="location-icon" style="color:var(--neon-cyan); background:rgba(0,229,255,0.1);"><i class="fas fa-map-marked-alt"></i></div>
                <div class="location-text">
                    <h4>Current Area Detected</h4>
                    <p id="a-loc-name">Resolving coordinates...</p>
                </div>
            </div>

            <button class="mn-btn btn-dark" style="border-color:var(--neon-cyan); color:var(--neon-cyan);" id="btn-broadcast" onclick="toggleLocationBroadcast(event)"><i class="fas fa-location-arrow"></i> INITIATE BROADCAST</button>
        </div>

        <div class="card">
            <h3 style="color:var(--neon-cyan); border-bottom-color:rgba(0, 229, 255, 0.3);"><i class="fas fa-bullhorn"></i> Public Logistics Feed</h3>
            
            <div class="quick-actions">
                <button class="quick-btn" onclick="setQuickUpdate('🚚 Fleet Dispatched. En route to venue.', event)"><i class="fas fa-truck-moving"></i> Dispatched</button>
                <button class="quick-btn" onclick="setQuickUpdate('📍 Fleet Arrived. Commencing Unloading.', event)"><i class="fas fa-map-marker-alt"></i> Arrived</button>
                <button class="quick-btn" onclick="setQuickUpdate('⚙️ Unloaded. Stage & Audio setup begun.', event)"><i class="fas fa-tools"></i> Setup</button>
                <button class="quick-btn" style="border-color:rgba(255,51,51,0.5); color:#ff6b6b;" onclick="setQuickUpdate('🛑 Minor delay due to traffic. Actively moving.', event)"><i class="fas fa-traffic-light"></i> Delay</button>
            </div>

            <textarea id="admin-update-text" class="mn-input" rows="2" style="resize:none; padding:12px; flex-shrink:0;" placeholder="Push an official update..."></textarea>
            <button class="mn-btn btn-gold" style="margin-top:10px; flex-shrink:0; background: linear-gradient(135deg, var(--neon-cyan) 0%, #0088cc 100%); box-shadow: 0 5px 25px rgba(0,229,255,0.3);" id="btn-push-update" onclick="adminPushUpdate(event)"><i class="fas fa-paper-plane"></i> TRANSMIT</button>
            
            <div style="border-top: 1px dashed rgba(255,255,255,0.1); padding-top:12px; margin-top: 12px;">
                <h4 style="font-size:10px; color:#aaa; margin-bottom:8px; text-transform:uppercase; flex-shrink:0;">Manage Sent Updates:</h4>
                <div id="admin-manage-updates-area" class="timeline-feed" style="max-height: 150px;">
                    </div>
            </div>
        </div>

        <div class="card" id="admin-route-card" style="display:none; border-color: var(--brand-purple); background:rgba(124,58,237,0.05); padding: 15px; width: 100%; max-width: 100%;">
            <h3 style="color:var(--brand-purple); border-bottom-color:rgba(124,58,237,0.3); margin-bottom:15px; font-size:16px;"><i class="fas fa-route"></i> Live Delivery Route</h3>
            
            <div class="map-wrapper" style="height: 350px; margin-bottom: 12px; border-color: rgba(124,58,237,0.3); border-width: 3px;">
                <iframe id="admin-route-iframe" class="map-iframe" style="pointer-events: auto !important;" src="" onload="this.classList.add('loaded')"></iframe>
            </div>
            
            <div style="display:flex; gap:8px; margin-top:10px;">
                <button class="mn-btn btn-dark" style="font-size:11px; padding:12px; flex:1; border-color:rgba(124,58,237,0.5); color:var(--brand-purple);" onclick="forceRefreshMap('admin')"><i class="fas fa-sync-alt"></i> REFRESH MAP</button>
                <a id="admin-route-ext-link" href="#" target="_blank" class="mn-btn btn-green" style="font-size:12px; padding:12px; text-decoration:none; flex:2; background: linear-gradient(135deg, var(--brand-purple) 0%, #9333ea 100%); box-shadow: 0 5px 20px rgba(124,58,237,0.4); border:none;"><i class="fas fa-external-link-alt"></i> OPEN IN GOOGLE MAPS</a>
            </div>
        </div>

        <div class="chat-card chat-minimized" id="admin-chat-container">
            <div class="chat-header">
                <div style="display:flex; align-items:center; gap:10px;">
                    <div style="width:35px; height:35px; border-radius:50%; background:var(--neon-cyan); display:flex; justify-content:center; align-items:center; color:#000; font-size:16px;"><i class="fas fa-user-tie"></i></div>
                    <div>
                        <h3 style="margin:0; padding:0; color:#fff; font-size:14px; border:none;" id="admin-chat-header-name">Target Client</h3>
                        <div id="admin-chat-status" style="font-size:10px; color:var(--neon-green); transition: 0.3s;">Online</div>
                    </div>
                </div>
                <div style="display:flex; align-items:center; gap:18px; color:var(--gold); font-size:18px;">
                    <i class="fas fa-expand-arrows-alt" id="admin-chat-expand" style="cursor:pointer; color:var(--neon-cyan);" onclick="expandChat('admin')"></i>
                    <i class="fas fa-compress-alt" id="admin-chat-compress" style="display:none; cursor:pointer; color:var(--neon-green);" onclick="minimizeChat('admin', event)"></i>
                    <i class="fas fa-desktop" style="cursor:pointer;" onclick="initCall('screen', 'admin', event)"></i>
                    <i class="fas fa-video" style="cursor:pointer;" onclick="initCall('video', 'admin', event)"></i>
                    <i class="fas fa-phone-alt" style="cursor:pointer;" onclick="initCall('audio', 'admin', event)"></i>
                </div>
            </div>
            
            <div id="admin-chat-area" class="chat-area" onclick="closeAllMenus()">
                <div style="font-size:11px; color:rgba(255,255,255,0.5); text-align:center; margin: auto; background:rgba(0,0,0,0.4); padding:5px 10px; border-radius:10px;">End-to-end encrypted chat started</div>
            </div>

            <div id="admin-reply-banner" class="reply-banner">
                <div class="reply-banner-content">
                    <div class="rep-title" id="admin-reply-sender">Client Name</div>
                    <div class="replying-to-text" id="admin-reply-text">Replying to...</div>
                </div>
                <button class="cancel-reply-btn" onclick="cancelReply('admin', event)"><i class="fas fa-times"></i></button>
            </div>

            <div class="chat-input-container">
                <div id="admin-sticker-menu" class="sticker-drawer">
                    <div class="sticker-grid">
                        <img src="https://cdn-icons-png.flaticon.com/512/190/190411.png" onclick="sendSticker('admin', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/190/190406.png" onclick="sendSticker('admin', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/190/190412.png" onclick="sendSticker('admin', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/4140/4140061.png" onclick="sendSticker('admin', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/190/190413.png" onclick="sendSticker('admin', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/4140/4140048.png" onclick="sendSticker('admin', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/4140/4140047.png" onclick="sendSticker('admin', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/4140/4140034.png" onclick="sendSticker('admin', this.src, event)">
                    </div>
                </div>

                <div id="admin-attach-menu" class="attachment-drawer">
                    <div class="attach-item" onclick="triggerFileInput('file-doc', 'admin')"><div class="attach-icon-circle bg-doc"><i class="fas fa-file-alt"></i></div><span>Document</span></div>
                    <div class="attach-item" onclick="triggerFileInput('file-camera', 'admin')"><div class="attach-icon-circle bg-cam"><i class="fas fa-camera"></i></div><span>Camera</span></div>
                    <div class="attach-item" onclick="triggerFileInput('file-gallery', 'admin')"><div class="attach-icon-circle bg-gal"><i class="fas fa-image"></i></div><span>Gallery</span></div>
                    <div class="attach-item" onclick="triggerFileInput('file-audio', 'admin')"><div class="attach-icon-circle bg-aud"><i class="fas fa-headphones"></i></div><span>Audio</span></div>
                    <div class="attach-item" onclick="sendLocationMessage('admin', event)"><div class="attach-icon-circle bg-loc"><i class="fas fa-map-marker-alt"></i></div><span>Location</span></div>
                    <div class="attach-item" onclick="sendContactMessage('admin', event)"><div class="attach-icon-circle bg-con"><i class="fas fa-user"></i></div><span>Contact</span></div>
                </div>

                <div class="chat-modern-pill" id="admin-default-pill">
                    <button class="modern-left-btn" onclick="toggleAttachMenu('admin', event)">
                        <i class="fas fa-camera" id="admin-left-icon"></i>
                    </button>
                    
                    <input type="text" id="admin-chat-input" class="modern-input" placeholder="Message Client..." oninput="handleInputTyping('admin')">
                    
                    <div class="modern-right-icons" id="admin-right-icons">
                        <i class="fas fa-microphone" onclick="startRecording('admin', event)"></i>
                        <i class="fas fa-image" onclick="triggerFileInput('file-gallery', 'admin', event)"></i>
                        <i class="far fa-sticky-note" onclick="toggleStickerMenu('admin', event)"></i>
                        <i class="fas fa-plus-circle" onclick="toggleAttachMenu('admin', event)"></i>
                    </div>
                    
                    <button id="admin-send-btn" class="modern-send-btn" onclick="adminSendChatMessage(event)">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>

                <div class="recording-ui" id="admin-recording-pill">
                    <div style="display:flex; align-items:center;">
                        <span class="blinking-dot"></span> <span class="rec-timer" id="admin-record-time">00:00</span>
                    </div>
                    <div style="display:flex; align-items:center; gap:8px;">
                        <button class="modern-left-btn" style="background:#555;" onclick="cancelRecording('admin', event)"><i class="fas fa-trash"></i></button>
                        <button class="modern-send-btn active" style="width:34px; margin:0;" onclick="stopAndSendRecording('admin', event)"><i class="fas fa-paper-plane"></i></button>
                    </div>
                </div>
            </div>
        </div>
        
        <div style="width: 100%; max-width: 600px; display: flex; flex-direction: column; gap: 10px; margin-top: 10px; flex-shrink: 0;">
            <button class="mn-btn btn-danger" onclick="systemLogout(event)">
                <i class="fas fa-power-off"></i> DISCONNECT & LOGOUT
            </button>
            <a href="https://maa-nirmala-dj.github.io/-tent-house./" target="_blank" class="mn-btn btn-dark" style="text-decoration: none;">
                <i class="fas fa-globe"></i> VISIT OFFICIAL WEBSITE
            </a>
            <a href="https://maa-nirmala-dj.github.io/WELCOME-TO-MND-HUB/" target="_blank" class="mn-btn btn-gold" style="text-decoration: none; background: linear-gradient(135deg, var(--brand-purple) 0%, #9333ea 100%); color:#fff; box-shadow: 0 5px 20px rgba(124,58,237,0.4); border:none;">
                <i class="fas fa-layer-group"></i> MND HUB PORTAL
            </a>
        </div>
        <div class="app-footer">Powered by Maa Nirmala DJ</div>
    </div>

    <div id="view-client" class="view-container" style="padding: 15px 10px;">
        <div class="dash-header" style="max-width:100%; width:100%;">
            <div style="width:65%; overflow: hidden;">
                <h2 id="client-dash-event" style="color:var(--gold); font-family:'Cinzel'; margin:0; font-size:16px; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">Loading Event...</h2>
                <span style="font-size:10px; color:#888;">Auth: <span id="client-dash-name" style="color:#fff;">Loading...</span></span>
            </div>
            <div style="display:flex; align-items:center; gap:12px;">
                <div class="status-badge" id="client-conn-status"><i class="fas fa-circle fa-beat"></i> ONLINE</div>
                <i class="fas fa-bell" style="color:var(--gold); font-size:18px; cursor:pointer;" onclick="openNotifDrawer()"></i>
                <i class="fas fa-cog" style="color:#aaa; font-size:18px; cursor:pointer;" onclick="openSettings()"></i>
            </div>
        </div>

        <div class="card" style="max-width:100%; width:100%; padding: 12px; background:rgba(0, 250, 154, 0.05); border-color:var(--neon-green); margin-bottom:12px; flex-shrink:0;">
            <div style="display:flex; justify-content:space-between; align-items:center;">
                <div>
                    <h4 style="color:var(--neon-green); margin:0; font-size:13px;"><i class="fas fa-street-view"></i> Event Location</h4>
                    <p style="font-size:10px; color:#aaa; margin:0;">Send map pin to logistics.</p>
                </div>
                <button class="mn-btn btn-green" style="width:auto; padding: 8px 12px; font-size:11px;" onclick="clientShareLocation(event)"><i class="fas fa-share-location"></i> SHARE</button>
            </div>
        </div>

        <div class="card" style="max-width:100%; width:100%; padding: 15px; flex-shrink:0; margin-bottom:15px;">
            <div style="display:flex; justify-content:space-between; align-items:center; padding-bottom: 12px; border-bottom: 1px dashed rgba(255,255,255,0.1); margin-bottom: 10px;">
                <h3 style="margin:0; border:none; padding:0; font-size:16px;"><i class="fas fa-crosshairs"></i> Live Tracking</h3>
                <span id="client-gps-time" style="font-size:9px; color:#888; font-family:'Orbitron';">Awaiting Signal...</span>
            </div>
            
            <div class="telemetrics-grid">
                <div class="metric-box"><div class="metric-label">Speed</div><div class="metric-value" id="c-speed">--</div></div>
                <div class="metric-box"><div class="metric-label">Heading</div><div class="metric-value" id="c-heading">--</div></div>
                <div class="metric-box"><div class="metric-label">Signal</div><div class="metric-value" id="c-accuracy" style="color:var(--gold);">--</div></div>
            </div>

            <div class="map-wrapper">
                <iframe id="client-map-iframe" class="map-iframe" src="" onload="this.classList.add('loaded')"></iframe>
            </div>

            <div class="location-display" id="client-location-display" style="display:none;">
                <div class="location-icon"><i class="fas fa-map-marker-alt"></i></div>
                <div class="location-text">
                    <h4>Nearest Detected Area</h4>
                    <p id="c-location-name">Syncing AI Location...</p>
                </div>
            </div>
            
            <a id="ext-map-link" href="#" target="_blank" class="mn-btn btn-green" style="font-size:12px; padding:12px; text-decoration:none; display:none;"><i class="fas fa-external-link-alt"></i> OPEN IN MAPS</a>
        </div>

        <div class="card" style="max-width:100%; width:100%; display:flex; flex-direction:column; flex-shrink:0;">
            <h3 style="margin-bottom: 15px; flex-shrink:0;"><i class="fas fa-bullhorn"></i> Official Logistics Updates</h3>
            <div id="client-updates-area" class="timeline-feed" style="max-height: 250px;">
                <div style="text-align:center; color:#555; font-size:12px; padding:20px; border:none; margin-left:-15px;"><i class="fas fa-circle-notch fa-spin"></i> Listening for broadcasts...</div>
            </div>
        </div>

        <div class="card" id="client-route-card" style="display:none; max-width:100%; width:100%; padding:15px; flex-shrink:0; border-color: var(--brand-purple); background:rgba(124,58,237,0.05);">
            <h3 style="color:var(--brand-purple); border-bottom-color:rgba(124,58,237,0.3); margin-bottom:15px; font-size:16px;"><i class="fas fa-route"></i> Live Delivery Route</h3>
            <div class="map-wrapper" style="height: 350px; margin-bottom: 12px; border-color: rgba(124,58,237,0.3); border-width: 3px;">
                <iframe id="client-route-iframe" class="map-iframe" style="pointer-events: auto !important;" src="" onload="this.classList.add('loaded')"></iframe>
            </div>
            <div style="display:flex; gap:8px; margin-top:10px;">
                <button class="mn-btn btn-dark" style="font-size:11px; padding:12px; flex:1; border-color:rgba(124,58,237,0.5); color:var(--brand-purple);" onclick="forceRefreshMap('client')"><i class="fas fa-sync-alt"></i> REFRESH MAP</button>
                <a id="client-route-ext-link" href="#" target="_blank" class="mn-btn btn-green" style="font-size:12px; padding:12px; text-decoration:none; flex:2; background: linear-gradient(135deg, var(--brand-purple) 0%, #9333ea 100%); box-shadow: 0 5px 20px rgba(124,58,237,0.4); border:none;"><i class="fas fa-external-link-alt"></i> OPEN IN GOOGLE MAPS</a>
            </div>
        </div>

        <div class="chat-card chat-minimized" id="client-chat-container">
            <div class="chat-header">
                <div style="display:flex; align-items:center; gap:10px;">
                    <div style="width:35px; height:35px; border-radius:50%; background:var(--gold); display:flex; justify-content:center; align-items:center; color:#000; font-size:16px;"><i class="fas fa-headset"></i></div>
                    <div>
                        <h3 style="margin:0; padding:0; color:#fff; font-size:14px; border:none;">HQ Dispatch Support</h3>
                        <div id="client-chat-status" style="font-size:10px; color:var(--neon-green); transition: 0.3s;">Online</div>
                    </div>
                </div>
                <div style="display:flex; align-items:center; gap:18px; color:var(--gold); font-size:18px;">
                    <i class="fas fa-expand-arrows-alt" id="client-chat-expand" style="cursor:pointer; color:var(--neon-cyan);" onclick="expandChat('client')"></i>
                    <i class="fas fa-compress-alt" id="client-chat-compress" style="display:none; cursor:pointer; color:var(--neon-cyan);" onclick="minimizeChat('client', event)"></i>
                    <i class="fas fa-desktop" style="cursor:pointer;" onclick="initCall('screen', 'client', event)"></i>
                    <i class="fas fa-video" style="cursor:pointer;" onclick="initCall('video', 'client', event)"></i>
                    <i class="fas fa-phone-alt" style="cursor:pointer;" onclick="initCall('audio', 'client', event)"></i>
                </div>
            </div>
            
            <div id="client-chat-area" class="chat-area" onclick="closeAllMenus()">
                <div style="font-size:11px; color:rgba(255,255,255,0.5); text-align:center; margin: auto; background:rgba(0,0,0,0.4); padding:5px 10px; border-radius:10px;">End-to-end encrypted chat started</div>
            </div>

            <div id="client-reply-banner" class="reply-banner">
                <div class="reply-banner-content">
                    <div class="rep-title" id="client-reply-sender">HQ Dispatch</div>
                    <div class="replying-to-text" id="client-reply-text">Replying to...</div>
                </div>
                <button class="cancel-reply-btn" onclick="cancelReply('client', event)"><i class="fas fa-times"></i></button>
            </div>

            <div class="chat-input-container">
                <div id="client-sticker-menu" class="sticker-drawer">
                    <div class="sticker-grid">
                        <img src="https://cdn-icons-png.flaticon.com/512/190/190411.png" onclick="sendSticker('client', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/190/190406.png" onclick="sendSticker('client', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/190/190412.png" onclick="sendSticker('client', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/4140/4140061.png" onclick="sendSticker('client', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/190/190413.png" onclick="sendSticker('client', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/4140/4140048.png" onclick="sendSticker('client', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/4140/4140047.png" onclick="sendSticker('client', this.src, event)">
                        <img src="https://cdn-icons-png.flaticon.com/512/4140/4140034.png" onclick="sendSticker('client', this.src, event)">
                    </div>
                </div>

                <div id="client-attach-menu" class="attachment-drawer">
                    <div class="attach-item" onclick="triggerFileInput('file-doc', 'client')"><div class="attach-icon-circle bg-doc"><i class="fas fa-file-alt"></i></div><span>Document</span></div>
                    <div class="attach-item" onclick="triggerFileInput('file-camera', 'client')"><div class="attach-icon-circle bg-cam"><i class="fas fa-camera"></i></div><span>Camera</span></div>
                    <div class="attach-item" onclick="triggerFileInput('file-gallery', 'client')"><div class="attach-icon-circle bg-gal"><i class="fas fa-image"></i></div><span>Gallery</span></div>
                    <div class="attach-item" onclick="triggerFileInput('file-audio', 'client')"><div class="attach-icon-circle bg-aud"><i class="fas fa-headphones"></i></div><span>Audio</span></div>
                    <div class="attach-item" onclick="sendLocationMessage('client', event)"><div class="attach-icon-circle bg-loc"><i class="fas fa-map-marker-alt"></i></div><span>Location</span></div>
                    <div class="attach-item" onclick="sendContactMessage('client', event)"><div class="attach-icon-circle bg-con"><i class="fas fa-user"></i></div><span>Contact</span></div>
                </div>

                <div class="chat-modern-pill" id="client-default-pill">
                    <button class="modern-left-btn" onclick="toggleAttachMenu('client', event)">
                        <i class="fas fa-camera" id="client-left-icon"></i>
                    </button>
                    
                    <input type="text" id="client-chat-input" class="modern-input" placeholder="Message HQ..." oninput="handleInputTyping('client')">
                    
                    <div class="modern-right-icons" id="client-right-icons">
                        <i class="fas fa-microphone" onclick="startRecording('client', event)"></i>
                        <i class="fas fa-image" onclick="triggerFileInput('file-gallery', 'client', event)"></i>
                        <i class="far fa-sticky-note" onclick="toggleStickerMenu('client', event)"></i>
                        <i class="fas fa-plus-circle" onclick="toggleAttachMenu('client', event)"></i>
                    </div>
                    
                    <button id="client-send-btn" class="modern-send-btn" onclick="clientSendChatMessage(event)">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>

                <div class="recording-ui" id="client-recording-pill">
                    <div style="display:flex; align-items:center;">
                        <span class="blinking-dot"></span> <span class="rec-timer" id="client-record-time">00:00</span>
                    </div>
                    <div style="display:flex; align-items:center; gap:8px;">
                        <button class="modern-left-btn" style="background:#555;" onclick="cancelRecording('client', event)"><i class="fas fa-trash"></i></button>
                        <button class="modern-send-btn active" style="width:34px; margin:0;" onclick="stopAndSendRecording('client', event)"><i class="fas fa-paper-plane"></i></button>
                    </div>
                </div>
            </div>
        </div>
        
        <div style="width: 100%; max-width: 600px; display: flex; flex-direction: column; gap: 10px; margin-top: 10px; flex-shrink: 0;">
            <button class="mn-btn btn-danger" onclick="systemLogout(event)">
                <i class="fas fa-power-off"></i> DISCONNECT & LOGOUT
            </button>
            <a href="https://maa-nirmala-dj.github.io/-tent-house./" target="_blank" class="mn-btn btn-dark" style="text-decoration: none;">
                <i class="fas fa-globe"></i> VISIT OFFICIAL WEBSITE
            </a>
            <a href="https://maa-nirmala-dj.github.io/WELCOME-TO-MND-HUB/" target="_blank" class="mn-btn btn-gold" style="text-decoration: none; background: linear-gradient(135deg, var(--brand-purple) 0%, #9333ea 100%); color:#fff; box-shadow: 0 5px 20px rgba(124,58,237,0.4); border:none;">
                <i class="fas fa-layer-group"></i> MND HUB PORTAL
            </a>
        </div>
        <div class="app-footer">Powered by Maa Nirmala DJ</div>
    </div>

    <script>
        // --- SETTINGS LOGIC ---
        let appSettings = {
            sound: localStorage.getItem('mnd_sound') !== 'false',
            vibration: localStorage.getItem('mnd_vibration') !== 'false'
        };

        function openSettings() {
            document.getElementById('toggle-sound').checked = appSettings.sound;
            document.getElementById('toggle-vibration').checked = appSettings.vibration;
            document.getElementById('settings-overlay').classList.add('active');
        }

        function closeSettings() {
            document.getElementById('settings-overlay').classList.remove('active');
        }

        function toggleSetting(key, val) {
            appSettings[key] = val;
            localStorage.setItem('mnd_' + key, val);
            if(val && appSettings.vibration && navigator.vibrate) navigator.vibrate(50);
        }

        function goHome() {
            closeSettings();
            if(currentAdminTargetPhone) {
                closeAdminSession();
            } else if(currentClientPhone) {
                // Client is already on home, just close settings
            } else {
                switchView('view-gatekeeper');
            }
        }

        // --- NOTIFICATION DRAWER & HISTORY LOGIC ---
        function openNotifDrawer() {
            document.getElementById('notification-drawer').classList.add('active');
            renderNotifications();
        }

        function closeNotifDrawer() {
            document.getElementById('notification-drawer').classList.remove('active');
        }
        
        window.deleteDrawerNotif = function(source, key, phoneToFetch) {
            if (confirm("Delete this notification?")) {
                if (source === 'global') {
                    db.ref(`global_alerts/${key}`).remove();
                } else {
                    db.ref(`trackings/${phoneToFetch}/notifications/${key}`).remove();
                }
                setTimeout(renderNotifications, 300);
            }
        };

        function renderNotifications() {
            const listArea = document.getElementById('notif-list-area');
            const phoneToFetch = document.getElementById('view-admin').classList.contains('active-view') || document.getElementById('view-admin-session').classList.contains('active-view') 
                ? 'admin_system' 
                : currentClientPhone;
                
            if(!phoneToFetch) return;

            Promise.all([
                db.ref(`trackings/${phoneToFetch}/notifications`).orderByChild('time').limitToLast(30).once('value'),
                db.ref('global_alerts').orderByChild('time').limitToLast(30).once('value')
            ]).then(([notifSnap, globalSnap]) => {
                let combined = [];
                if(notifSnap.exists()) {
                    notifSnap.forEach(child => combined.push({key: child.key, source: 'local', ...child.val()}));
                }
                if(globalSnap.exists()) {
                    globalSnap.forEach(child => {
                        const val = child.val();
                        combined.push({
                            key: child.key, 
                            source: 'global',
                            title: "Global Broadcast",
                            body: val.body,
                            type: 'global',
                            time: val.time
                        });
                    });
                }
                
                combined = combined.filter(n => !(n.type === 'global' && n.source === 'local'));
                combined.sort((a, b) => b.time - a.time);

                if(combined.length > 0) {
                    let html = '';
                    combined.slice(0, 30).forEach(n => {
                        const icon = n.type === 'login' ? 'fa-sign-in-alt' : (n.type === 'global' ? 'fa-bullhorn' : 'fa-bell');
                        const color = n.type === 'login' ? 'var(--neon-green)' : (n.type === 'global' ? 'var(--danger)' : 'var(--gold)');
                        const delBtn = phoneToFetch === 'admin_system' ? `<i class="fas fa-trash" style="color:var(--danger); cursor:pointer; margin-left:auto; padding:5px;" onclick="deleteDrawerNotif('${n.source}', '${n.key}', '${phoneToFetch}')"></i>` : '';
                        
                        html += `
                        <div style="background: rgba(255,255,255,0.05); padding: 12px; border-radius: 8px; border-left: 3px solid ${color}; display: flex; gap: 12px; align-items: flex-start; margin-bottom:10px;">
                            <div style="color: ${color}; font-size: 16px; margin-top: 2px;"><i class="fas ${icon}"></i></div>
                            <div style="flex-grow:1;">
                                <h4 style="margin: 0 0 3px 0; font-size: 13px; color: #fff;">${escapeHTML(n.title)}</h4>
                                <p style="margin: 0; font-size: 11px; color: #ccc;">${escapeHTML(n.body)}</p>
                                <span style="font-size: 9px; color: #888; display: block; margin-top: 5px;">${new Date(n.time).toLocaleString()}</span>
                            </div>
                            ${delBtn}
                        </div>`;
                    });
                    listArea.innerHTML = html;
                } else {
                    listArea.innerHTML = '<div style="text-align:center; color:#555; font-size:12px; padding:20px;">No recent alerts.</div>';
                }
            });
        }

        function logNotification(phone, title, body, type) {
            db.ref(`trackings/${phone}/notifications`).push({
                title: title, body: body, type: type, time: Date.now()
            });
        }

        // --- ADMIN GLOBAL BROADCAST ---
        function sendGlobalAlert() {
            const msg = document.getElementById('admin-global-msg').value.trim();
            if(!msg) { showToast("Enter a message to broadcast.", "error"); return; }
            
            db.ref('global_alerts').push({
                body: msg, time: Date.now()
            }).then(() => {
                showToast("Global Alert Transmitted to All Users!", "success");
                document.getElementById('admin-global-msg').value = '';
                logAdminNotification("Global Broadcast", msg);
            }).catch(e => showToast("Broadcast Failed", "error"));
        }

        function startGlobalHistoryListener() {
            db.ref('global_alerts').orderByChild('time').limitToLast(20).on('value', (snap) => {
                const list = document.getElementById('admin-global-history-list');
                if(!list) return;
                if(snap.exists()) {
                    let html = '';
                    const alerts = [];
                    snap.forEach(child => alerts.push({key: child.key, ...child.val()}));
                    alerts.reverse().forEach(a => {
                        const safeText = escapeHTML(a.body);
                        html += `
                            <div class="timeline-item" style="padding-bottom:10px; margin-bottom:10px; border-left-color: var(--danger);">
                                <div class="update-content" style="width:75%;">
                                    <div class="update-time" style="color:var(--danger);">${new Date(a.time).toLocaleString()}</div>
                                    <div class="update-text">${safeText}</div>
                                </div>
                                <div style="display:flex; gap:10px; align-items:center;">
                                    <button class="action-icon-btn" style="color:var(--neon-cyan);" onclick="editGlobalAlert('${a.key}', '${safeText.replace(/'/g, "\\'")}')"><i class="fas fa-edit"></i></button>
                                    <button class="action-icon-btn trash" onclick="deleteGlobalAlert('${a.key}')"><i class="fas fa-trash"></i></button>
                                </div>
                            </div>
                        `;
                    });
                    list.innerHTML = html;
                } else {
                    list.innerHTML = '<div style="text-align:center; color:#555; font-size:12px; padding:10px;">No global alerts.</div>';
                }
            });
        }

        function editGlobalAlert(key, oldText) {
            const newText = prompt("Edit Global Alert:", oldText);
            if(newText !== null && newText.trim() !== '') {
                db.ref(`global_alerts/${key}`).update({body: newText.trim()})
                  .then(() => showToast("Global alert updated", "success"));
            }
        }

        function deleteGlobalAlert(key) {
            if(confirm("Delete this global broadcast?")) {
                db.ref(`global_alerts/${key}`).remove()
                  .then(() => showToast("Global alert deleted", "success"));
            }
        }

        let isGlobalAlertListenerActive = false;
        let processedGlobalAlerts = new Set();
        let initGlobalTime = Date.now();

        function startGlobalAlertListener(phoneToLog) {
            if(isGlobalAlertListenerActive) return;
            isGlobalAlertListenerActive = true;
            
            db.ref('global_alerts').on('child_added', snap => {
                const alert = snap.val();
                const key = snap.key;
                
                if(alert.time > initGlobalTime && !processedGlobalAlerts.has(key)) {
                    processedGlobalAlerts.add(key);
                    showPushNotification("HQ EMERGENCY BROADCAST", alert.body, 'message');
                    if (phoneToLog && phoneToLog !== 'admin_system') {
                        logNotification(phoneToLog, "Global Broadcast", alert.body, "global");
                    }
                }
            });
        }

        // --- MAP REFRESH LOGIC ---
        function forceRefreshMap(role) {
            const iframe = document.getElementById(role + '-route-iframe');
            if(iframe && iframe.src) {
                const currentSrc = iframe.src;
                iframe.src = '';
                setTimeout(() => { 
                    iframe.src = currentSrc; 
                    showToast("Map routing engine refreshed.");
                }, 100);
            } else {
                showToast("Map not loaded yet.", "error");
            }
        }

        // --- PREMIUM MEDIA VIEWER LOGIC ---
        function openPremiumMediaViewer(url, type, name) {
            const viewer = document.getElementById('premium-media-viewer');
            const container = document.getElementById('media-content-container');
            const title = document.getElementById('media-title');
            const downloadBtn = document.getElementById('media-download-btn');
            
            container.innerHTML = ''; // clear previous
            title.innerText = name ? decodeURIComponent(name) : (type === 'image' ? 'Image Media' : 'Document');
            downloadBtn.href = url;
            downloadBtn.download = name || (type === 'image' ? 'image.jpg' : 'document.pdf');
            
            if (type === 'image') {
                container.innerHTML = `<img src="${url}" />`;
            } else if (type === 'document') {
                container.innerHTML = `<embed src="${url}" type="application/pdf" style="width:100%; height:100%; border-radius:12px; background:white;"></embed>`;
            }
            
            viewer.style.display = 'flex';
        }

        function closePremiumMediaViewer() {
            const viewer = document.getElementById('premium-media-viewer');
            const container = document.getElementById('media-content-container');
            viewer.style.display = 'none';
            container.innerHTML = '';
        }

        // --- TRUE FULLSCREEN CHAT LOGIC (100% GAPLESS) ---
        function expandChat(role) {
            const container = document.getElementById(role + '-chat-container');
            const view = document.getElementById(role === 'admin' ? 'view-admin-session' : 'view-client');
            
            if (!container.classList.contains('chat-fullscreen')) {
                // Safely hide all siblings to allow chat to naturally fill 100% of the screen seamlessly
                Array.from(view.children).forEach(child => {
                    if (child.id !== role + '-chat-container' && child.tagName !== 'SCRIPT' && child.tagName !== 'STYLE') {
                        child.setAttribute('data-original-display', child.style.display);
                        child.setAttribute('data-hidden-by-chat', 'true');
                        child.style.setProperty('display', 'none', 'important');
                    }
                });
                
                view.style.padding = '0'; // Remove default view padding
                container.style.margin = '0'; 
                container.style.borderRadius = '0';
                
                container.classList.remove('chat-minimized');
                container.classList.add('chat-fullscreen');
                
                document.body.classList.add('chat-locked'); 
                document.getElementById(role + '-chat-expand').style.display = 'none';
                document.getElementById(role + '-chat-compress').style.display = 'inline-block';
                
                const area = document.getElementById(role + '-chat-area');
                setTimeout(() => { area.scrollTop = area.scrollHeight; }, 50);
            }
        }
        
        function minimizeChat(role, e) {
            if(e) e.stopPropagation();
            const container = document.getElementById(role + '-chat-container');
            const view = document.getElementById(role === 'admin' ? 'view-admin-session' : 'view-client');
            
            // Restore siblings
            Array.from(view.children).forEach(child => {
                if (child.hasAttribute('data-hidden-by-chat')) {
                    child.style.display = child.getAttribute('data-original-display') || '';
                    child.removeAttribute('data-hidden-by-chat');
                    child.removeAttribute('data-original-display');
                }
            });

            view.style.padding = '';
            container.style.margin = '';
            container.style.borderRadius = '';

            container.classList.remove('chat-fullscreen');
            container.classList.add('chat-minimized');
            
            document.body.classList.remove('chat-locked');
            document.getElementById(role + '-chat-compress').style.display = 'none';
            document.getElementById(role + '-chat-expand').style.display = 'inline-block';
        }

        // --- AUTO-LOGIN SYSTEM ---
        window.addEventListener('DOMContentLoaded', () => {
            const savedPhone = localStorage.getItem('mnd_auth_phone');
            const savedPin = localStorage.getItem('mnd_auth_pin');
            if (savedPhone && savedPin) {
                document.getElementById('login-phone').value = savedPhone;
                document.getElementById('login-pin').value = savedPin;
                setTimeout(() => processLogin(), 300);
            }
        });

        // --- CLEAR LOGIN INPUTS ---
        function clearLogin(e) {
            if(e) e.stopPropagation();
            document.getElementById('login-phone').value = '';
            document.getElementById('login-pin').value = '';
            document.getElementById('login-error').style.display = 'none';
            const btn = document.getElementById('login-btn');
            btn.innerHTML = '<i class="fas fa-fingerprint"></i> INITIATE HANDSHAKE';
            btn.disabled = false;
        }

        // --- PWA & APP INSTALL LOGIC ---
        const manifestData = {
            "name": "MND Logistics Live Tracking",
            "short_name": "MND Track",
            "start_url": ".",
            "display": "standalone",
            "background_color": "#ffffff",
            "theme_color": "#ffffff",
            "icons": [{"src": "https://i.postimg.cc/52vLtJBM/1000095487-(2).png", "sizes": "512x512", "type": "image/png"}]
        };
        const manifestBlob = new Blob([JSON.stringify(manifestData)], {type: 'application/manifest+json'});
        const manifestLink = document.createElement('link');
        manifestLink.rel = 'manifest';
        manifestLink.href = URL.createObjectURL(manifestBlob);
        document.head.appendChild(manifestLink);

        let deferredPrompt;
        
        window.addEventListener('beforeinstallprompt', (e) => {
            e.preventDefault(); 
            deferredPrompt = e;
            
            if (!window.matchMedia('(display-mode: standalone)').matches) {
                document.getElementById('install-banner').classList.add('show');
                
                setTimeout(() => {
                   if(document.getElementById('install-banner').classList.contains('show')) {
                       showPushNotification("App Available", "Install the MND Tracking App for a seamless fullscreen experience.");
                   }
                }, 5000);
            }
        });

        async function installApp() {
            if (deferredPrompt) {
                deferredPrompt.prompt();
                const { outcome } = await deferredPrompt.userChoice;
                if (outcome === 'accepted') {
                    showPushNotification("Installing...", "MND Tracking is being added to your device.");
                }
                deferredPrompt = null;
                closeInstallBanner();
            } else {
                window.open('https://maa-nirmala-dj.github.io/-tent-house./', '_blank');
            }
        }
        
        window.addEventListener('appinstalled', () => {
            deferredPrompt = null;
            closeInstallBanner();
            showPushNotification("Installation Complete", "MND Tracking is now natively installed on your device.");
        });
        
        function closeInstallBanner() {
            document.getElementById('install-banner').classList.remove('show');
        }

        // --- DISPATCH MULTI-PLATFORM & LANGUAGE LOGIC ---
        let dispatchLang = 'en';

        function openDispatchModal(e) {
            if(e) e.stopPropagation();
            document.getElementById('dispatch-overlay').classList.add('active');
        }

        function closeDispatchModal() {
            document.getElementById('dispatch-overlay').classList.remove('active');
        }

        function toggleDispatchLang(lang) {
            dispatchLang = lang;
            document.getElementById('lang-btn-en').style.opacity = lang === 'en' ? '1' : '0.5';
            document.getElementById('lang-btn-hi').style.opacity = lang === 'hi' ? '1' : '0.5';
        }

        function generateDispatchMsg() {
            if(!activeClientData) return "";
            if(dispatchLang === 'en') {
                return `*👑 MAA NIRMALA DJ & TENT HOUSE BELTIKRI ( 813106 ) 👑*\n\n *📍 LIVE LOCATION TRACKING📍*\n\nHello ${activeClientData.name},\nYour logistics for *${activeClientData.event}* are active.\n----------------------------------------------------------\n*How to track:*\n1. Click the link and open the website.\n2. At the bottom right corner (above the MND AI button), click the 'Live Location' button to open the tracking portal.\n3. Enter your registered mobile number and 6-digit PIN to track.\n----------------------------------------------------------\n •••••••••••••••••••••••••••••••••••••••••••••••\n 🔗 *Link:* https://maa-nirmala-dj.github.io/-tent-house./\n📱 *Login Number:* ${activeClientData.phone}\n🔐 *Tracking PIN:* ${activeClientData.pin}\n•••••••••••••••••••••••••••••••••••••••••••••••\nThank You.`;
            } else {
                return `*👑 माँ निर्मला डीजे एंड टेंट हाउस बेलटिकरी (813106) 👑*\n\n *📍 लाइव लोकेशन ट्रैकिंग 📍*\n\nनमस्ते ${activeClientData.name},\n*${activeClientData.event}* के लिए आपका लॉजिस्टिक्स सक्रिय है।\n----------------------------------------------------------\n*ट्रैक कैसे करें:*\n1. लिंक पर क्लिक करें और वेबसाइट खोलें।\n2. नीचे दाएं कोने में (MND AI बटन के ऊपर), ट्रैकिंग पोर्टल खोलने के लिए 'लाइव लोकेशन' बटन पर क्लिक करें।\n3. ट्रैक करने के लिए अपना पंजीकृत मोबाइल नंबर और 6-अंकीय पिन दर्ज करें।\n----------------------------------------------------------\n •••••••••••••••••••••••••••••••••••••••••••••••\n 🔗 *लिंक:* https://maa-nirmala-dj.github.io/-tent-house./\n📱 *लॉगिन नंबर:* ${activeClientData.phone}\n🔐 *ट्रैकिंग पिन:* ${activeClientData.pin}\n•••••••••••••••••••••••••••••••••••••••••••••••\nधन्यवाद।`;
            }
        }

        function dispatchVia(platform) {
            if(!activeClientData) return;
            const msg = generateDispatchMsg();
            const cleanPhone = activeClientData.phone.replace(/\D/g, '');
            
            if(platform === 'whatsapp') {
                window.open(`https://wa.me/91${cleanPhone}?text=${encodeURIComponent(msg)}`, '_blank');
            } else if(platform === 'sms') {
                window.open(`sms:+91${cleanPhone}?body=${encodeURIComponent(msg)}`, '_self');
            } else if(platform === 'instagram' || platform === 'facebook') {
                navigator.clipboard.writeText(msg).then(() => {
                    showToast(`Message copied! Open ${platform} to paste & send.`);
                    setTimeout(() => {
                        if(platform === 'instagram') window.open('https://www.instagram.com/direct/inbox/', '_blank');
                        if(platform === 'facebook') window.open('https://www.messenger.com/', '_blank');
                    }, 1500);
                });
            }
            closeDispatchModal();
        }

        function escapeHTML(str) {
            if (!str) return "";
            return String(str).replace(/[&<>'"]/g, 
                tag => ({
                    '&': '&amp;',
                    '<': '&lt;',
                    '>': '&gt;',
                    "'": '&#39;',
                    '"': '&quot;'
                }[tag] || tag)
            );
        }

        function closeAllMenus() {
            document.querySelectorAll('.attachment-drawer').forEach(d => d.classList.remove('show'));
            document.querySelectorAll('.sticker-drawer').forEach(d => d.classList.remove('show'));
        }

        // --- MODERN PILL INPUT LOGIC ---
        function handleInputTyping(role) {
            const input = document.getElementById(role + '-chat-input');
            const rightIcons = document.getElementById(role + '-right-icons');
            const sendBtn = document.getElementById(role + '-send-btn');
            const leftIcon = document.getElementById(role + '-left-icon');
            
            if (input.value.trim().length > 0) {
                rightIcons.classList.add('hidden');
                sendBtn.classList.add('active');
                leftIcon.className = 'fas fa-search';
            } else {
                rightIcons.classList.remove('hidden');
                sendBtn.classList.remove('active');
                leftIcon.className = 'fas fa-camera';
            }
        }

        function toggleAttachMenu(role, e) {
            if(e) e.stopPropagation();
            closeAllMenus();
            const drawer = document.getElementById(role + '-attach-menu');
            drawer.classList.toggle('show');
        }

        function toggleStickerMenu(role, e) {
            if(e) e.stopPropagation();
            closeAllMenus();
            const drawer = document.getElementById(role + '-sticker-menu');
            drawer.classList.toggle('show');
        }

        // --- CHAT FUNCTIONS ---
        function copyChatText(text, e) {
            if(e) e.stopPropagation();
            const rawText = text.replace(/"/g, '"').replace(/'/g, "'");
            navigator.clipboard.writeText(rawText).then(() => {
                showToast("Copied to clipboard!");
                closeMsgModal(); 
            }).catch(err => {
                showToast("Failed to copy", "error");
            });
        }

        function initiateReply(key, text, originalSenderRole, role, e) {
            if(e) e.stopPropagation();
            closeAllMenus();
            if (typeof closeMsgModal === 'function') closeMsgModal();
            
            let rawText = text.replace(/"/g, '"').replace(/'/g, "'");
            let shortText = rawText.length > 40 ? rawText.substring(0, 40) + '...' : rawText;
            
            if(role === 'admin') {
                const senderName = originalSenderRole === 'admin' ? 'HQ Dispatch' : document.getElementById('session-c-name').innerText;
                adminReplyContext = { key, text: shortText, sender: originalSenderRole };
                document.getElementById('admin-reply-sender').innerText = senderName;
                document.getElementById('admin-reply-text').innerText = escapeHTML(shortText);
                document.getElementById('admin-reply-banner').style.display = 'flex';
                document.getElementById('admin-chat-input').focus();
            } else {
                const senderName = originalSenderRole === 'admin' ? 'HQ Dispatch' : currentClientName;
                clientReplyContext = { key, text: shortText, sender: originalSenderRole };
                document.getElementById('client-reply-sender').innerText = senderName;
                document.getElementById('client-reply-text').innerText = escapeHTML(shortText);
                document.getElementById('client-reply-banner').style.display = 'flex';
                document.getElementById('client-chat-input').focus();
            }
        }

        function cancelReply(role, e) {
            if(e) e.stopPropagation();
            if(role === 'admin') {
                adminReplyContext = null;
                const banner = document.getElementById('admin-reply-banner');
                if(banner) banner.style.display = 'none';
            } else {
                clientReplyContext = null;
                const banner = document.getElementById('client-reply-banner');
                if(banner) banner.style.display = 'none';
            }
        }

        function deleteChatMessage(targetPhone, key, e) {
            if(e) e.stopPropagation();
            if(!targetPhone || !key) return;
            
            db.ref(`trackings/${targetPhone}/client_messages/${key}`).remove()
            .then(() => {
                if (typeof closeMsgModal === 'function') closeMsgModal();
            })
            .catch(err => showToast("Failed to delete", "error"));
        }

        function adminSendChatMessage(e) {
            if(e) e.stopPropagation();
            if(!currentAdminTargetPhone) return;
            
            const input = document.getElementById('admin-chat-input');
            const text = input.value.trim();
            if(!text) return;

            input.value = '';
            handleInputTyping('admin');

            const msgData = {
                text: text, sender: 'admin',
                time: new Date().toLocaleTimeString('en-US', {hour: '2-digit', minute:'2-digit'}),
                timestamp: Date.now(),
                status: 'sent',
                readTime: ''
            };

            if(adminReplyContext) {
                msgData.replyToKey = adminReplyContext.key;
                msgData.replyToText = adminReplyContext.text;
                msgData.replyToSender = adminReplyContext.sender;
            }
            
            cancelReply('admin'); 

            db.ref(`trackings/${currentAdminTargetPhone}/client_messages`).push(msgData).then(() => {
                setTimeout(() => {
                    const area = document.getElementById('admin-chat-area');
                    area.scrollTop = area.scrollHeight;
                }, 50);
            }).catch(e => showToast("Send failed", "error"));
        }

        function clientSendChatMessage(e) {
            if(e) e.stopPropagation();
            if(!currentClientPhone) return;
            
            const input = document.getElementById('client-chat-input');
            const text = input.value.trim();
            if(!text) return;

            input.value = '';
            handleInputTyping('client');

            const msgData = {
                text: text, sender: 'client',
                time: new Date().toLocaleTimeString('en-US', {hour: '2-digit', minute:'2-digit'}),
                timestamp: Date.now(),
                status: 'sent',
                readTime: ''
            };

            if(clientReplyContext) {
                msgData.replyToKey = clientReplyContext.key;
                msgData.replyToText = clientReplyContext.text;
                msgData.replyToSender = clientReplyContext.sender;
            }
            
            cancelReply('client'); 

            db.ref(`trackings/${currentClientPhone}/client_messages`).push(msgData).then(() => {
                setTimeout(() => {
                    const area = document.getElementById('client-chat-area');
                    area.scrollTop = area.scrollHeight;
                }, 50);
            }).catch(e => showToast("Send failed", "error"));
        }

        // --- MEDIA & ATTACHMENT FUNCTIONS ---
        let activeUploadRole = null;

        function triggerFileInput(inputId, role, e) {
            if(e) e.stopPropagation();
            activeUploadRole = role;
            document.getElementById(inputId).click();
            closeAllMenus();
        }

        function handleFileUpload(event, type) {
            const file = event.target.files[0];
            if(!file || !activeUploadRole) return;
            
            if(file.size > 3 * 1024 * 1024 && type !== 'image' && type !== 'camera') { 
                showToast("File is too large! Please select under 3MB.", "error");
                return;
            }

            const reader = new FileReader();
            
            if((type === 'image' || type === 'camera') && file.type.startsWith('image/')) {
                reader.readAsDataURL(file);
                reader.onload = e => {
                    const img = new Image();
                    img.src = e.target.result;
                    img.onload = () => {
                        const canvas = document.createElement('canvas');
                        const MAX_WIDTH = 800;
                        let scale = 1;
                        if(img.width > MAX_WIDTH) scale = MAX_WIDTH / img.width;
                        canvas.width = img.width * scale;
                        canvas.height = img.height * scale;
                        const ctx = canvas.getContext('2d');
                        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                        const compressedBase64 = canvas.toDataURL('image/jpeg', 0.6);
                        sendMediaMessage(activeUploadRole, type, compressedBase64, { fileName: file.name });
                    };
                };
            } else {
                reader.readAsDataURL(file);
                reader.onloadend = () => {
                    sendMediaMessage(activeUploadRole, type, reader.result, { fileName: file.name });
                };
            }
            event.target.value = '';
        }

        function sendContactMessage(role, e) {
            if(e) e.stopPropagation();
            closeAllMenus();
            const name = prompt("Enter Contact Name:");
            if(!name) return;
            const phone = prompt("Enter Contact Number:");
            if(!phone) return;
            sendMediaMessage(role, 'contact', null, { contactName: name, contactPhone: phone });
        }

        function sendLocationMessage(role, e) {
            if(e) e.stopPropagation();
            closeAllMenus();
            if(!navigator.geolocation) { showToast("GPS not supported.", "error"); return; }
            showToast("Acquiring Live Location...");
            navigator.geolocation.getCurrentPosition((pos) => {
                sendMediaMessage(role, 'location', null, { lat: pos.coords.latitude, lng: pos.coords.longitude });
            }, () => { showToast("Failed to get location.", "error"); }, { enableHighAccuracy: true, timeout: 15000, maximumAge: 0 });
        }

        function sendSticker(role, src, e) {
            if(e) e.stopPropagation();
            closeAllMenus();
            sendMediaMessage(role, 'sticker', src);
        }

        function clientShareLocation(e) {
            if(e) e.stopPropagation();
            if(!currentClientPhone) return;
            if(!navigator.geolocation) { showToast("GPS not supported.", "error"); return; }
            
            showToast("Acquiring Event Location...");
            navigator.geolocation.getCurrentPosition((pos) => {
                db.ref(`trackings/${currentClientPhone}/client_location`).set({
                    lat: pos.coords.latitude,
                    lng: pos.coords.longitude,
                    timestamp: Date.now()
                }).then(() => {
                    showToast("Event Location Shared with HQ!");
                }).catch(() => {
                    showToast("Failed to share location.", "error");
                });
            }, (err) => { 
                showToast("Failed to get location. Ensure GPS is enabled.", "error"); 
            }, { enableHighAccuracy: true, timeout: 15000, maximumAge: 0 });
        }

        // --- VOICE RECORDER ---
        let mediaRecorder;
        let audioChunks = [];
        let recordInterval;
        let recordSeconds = 0;

        async function startRecording(role, e) {
            if(e) e.stopPropagation();
            closeAllMenus();
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
                mediaRecorder = new MediaRecorder(stream);
                audioChunks = [];
                
                document.getElementById(role + '-default-pill').style.display = 'none';
                document.getElementById(role + '-recording-pill').style.display = 'flex';
                
                recordSeconds = 0;
                document.getElementById(role + '-record-time').innerText = "00:00";
                recordInterval = setInterval(() => {
                    recordSeconds++;
                    const mins = String(Math.floor(recordSeconds / 60)).padStart(2, '0');
                    const secs = String(recordSeconds % 60).padStart(2, '0');
                    document.getElementById(role + '-record-time').innerText = `${mins}:${secs}`;
                }, 1000);

                mediaRecorder.ondataavailable = event => {
                    if (event.data.size > 0) audioChunks.push(event.data);
                };

                mediaRecorder.start();
            } catch (err) {
                showToast("Microphone access denied or not available.", "error");
            }
        }

        function cancelRecording(role, e) {
            if(e) e.stopPropagation();
            if(mediaRecorder && mediaRecorder.state !== 'inactive') {
                mediaRecorder.stop();
                mediaRecorder.stream.getTracks().forEach(track => track.stop());
            }
            clearInterval(recordInterval);
            document.getElementById(role + '-recording-pill').style.display = 'none';
            document.getElementById(role + '-default-pill').style.display = 'flex';
        }

        function stopAndSendRecording(role, e) {
            if(e) e.stopPropagation();
            if(mediaRecorder && mediaRecorder.state !== 'inactive') {
                mediaRecorder.onstop = () => {
                    const audioBlob = new Blob(audioChunks, { type: 'audio/webm' });
                    const reader = new FileReader();
                    reader.readAsDataURL(audioBlob);
                    reader.onloadend = () => {
                        sendMediaMessage(role, 'audio', reader.result);
                    };
                    mediaRecorder.stream.getTracks().forEach(track => track.stop());
                };
                mediaRecorder.stop();
            }
            clearInterval(recordInterval);
            document.getElementById(role + '-recording-pill').style.display = 'none';
            document.getElementById(role + '-default-pill').style.display = 'flex';
        }

        // --- SCREEN WAKE LOCK MANAGER ---
        let wakeLock = null;
        async function acquireWakeLock() {
            if ('wakeLock' in navigator) {
                try { wakeLock = await navigator.wakeLock.request('screen'); } catch (err) {}
            }
        }
        function releaseWakeLock() {
            if (wakeLock !== null) {
                wakeLock.release().then(() => { wakeLock = null; });
            }
        }

        // --- FULL WEBRTC LIVE CALLING & SCREEN SHARE ---
        const RTC = {
            pc: null,
            localStream: null,
            remoteStream: null,
            role: null, 
            isCaller: false,
            callRef: null,
            targetPhone: null
        };
        const servers = {
            iceServers: [
                { urls: ['stun:stun1.l.google.com:19302', 'stun:stun2.l.google.com:19302', 'stun:stun.l.google.com:19302', 'stun:global.stun.twilio.com:3478'] }
            ]
        };

        let activeCallValueListener = null;
        let iceCallerListener = null;
        let iceCalleeListener = null;
        let globalCallListenerRef = null;
        let globalCallListenerCb = null;

        let ringbackInterval = null;
        let ringerInterval = null;
        let audioCtx = null;
        
        let currentFacingMode = 'user'; 

        function playRingbackTone() {
            if(audioCtx || !appSettings.sound) return;
            try {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                ringbackInterval = setInterval(() => {
                    if(!audioCtx) return;
                    const osc1 = audioCtx.createOscillator();
                    const osc2 = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    
                    osc1.type = 'sine'; osc2.type = 'sine';
                    osc1.frequency.setValueAtTime(440, audioCtx.currentTime);
                    osc2.frequency.setValueAtTime(480, audioCtx.currentTime);
                    
                    gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 1.8);
                    
                    osc1.connect(gain); osc2.connect(gain); gain.connect(audioCtx.destination);
                    osc1.start(); osc2.start();
                    osc1.stop(audioCtx.currentTime + 1.8); osc2.stop(audioCtx.currentTime + 1.8);
                }, 3000);
            } catch(e) {}
        }

        function stopRingbackTone() {
            if(ringbackInterval) { clearInterval(ringbackInterval); ringbackInterval = null; }
            if(audioCtx) { audioCtx.close(); audioCtx = null; }
        }

        function playRingtone() {
            if(appSettings.vibration && navigator.vibrate) navigator.vibrate([1000, 500, 1000, 500, 1000]); 
            if(audioCtx || !appSettings.sound) return;
            try {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                ringerInterval = setInterval(() => {
                    if(!audioCtx) return;
                    if(appSettings.vibration && navigator.vibrate) navigator.vibrate([1000, 500, 1000, 500, 1000]);
                    
                    const osc = audioCtx.createOscillator(); const gain = audioCtx.createGain();
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(550, audioCtx.currentTime);
                    osc.frequency.linearRampToValueAtTime(650, audioCtx.currentTime + 0.1);
                    osc.frequency.linearRampToValueAtTime(550, audioCtx.currentTime + 0.2);
                    osc.frequency.linearRampToValueAtTime(650, audioCtx.currentTime + 0.3);
                    osc.frequency.linearRampToValueAtTime(550, audioCtx.currentTime + 0.4);
                    
                    gain.gain.setValueAtTime(0.2, audioCtx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 1.5);
                    
                    osc.connect(gain); gain.connect(audioCtx.destination);
                    osc.start(); osc.stop(audioCtx.currentTime + 1.5);
                }, 4000);
            } catch(e) {}
        }

        function stopRingtone() {
            if(navigator.vibrate) navigator.vibrate(0);
            if(ringerInterval) { clearInterval(ringerInterval); ringerInterval = null; }
            if(audioCtx) { audioCtx.close(); audioCtx = null; }
        }

        async function initCall(type, myRole, e) {
            if(e) e.stopPropagation();
            closeAllMenus();
            RTC.role = myRole;
            RTC.isCaller = true;
            RTC.targetPhone = myRole === 'admin' ? currentAdminTargetPhone : currentClientPhone;
            
            if(!RTC.targetPhone) return showToast("No active session target found.", "error");
            
            RTC.callRef = db.ref(`trackings/${RTC.targetPhone}/call`);
            RTC.callRef.onDisconnect().remove();

            document.getElementById('active-call-overlay').style.display = 'flex';
            document.getElementById('active-call-name').innerText = myRole === 'admin' ? document.getElementById('session-c-name').innerText : 'HQ Dispatch';
            document.getElementById('active-call-status').innerText = 'Calling...';

            document.getElementById('btn-toggle-mic').innerHTML = '<i class="fas fa-microphone"></i>';
            document.getElementById('btn-toggle-mic').style.background = 'rgba(255,255,255,0.2)';
            document.getElementById('btn-toggle-cam').innerHTML = '<i class="fas fa-video"></i>';
            document.getElementById('btn-toggle-cam').style.background = 'rgba(255,255,255,0.2)';

            if(type === 'audio') document.getElementById('local-video').style.display = 'none';
            else document.getElementById('local-video').style.display = 'block';

            try {
                if (type === 'screen') {
                    try {
                        RTC.localStream = await navigator.mediaDevices.getDisplayMedia({ 
                            video: true, 
                            audio: false
                        });
                        
                        try {
                            const micStream = await navigator.mediaDevices.getUserMedia({ audio: { echoCancellation: true, noiseSuppression: true, autoGainControl: true } });
                            const audioTrack = micStream.getAudioTracks()[0];
                            if(audioTrack) {
                                RTC.localStream.addTrack(audioTrack);
                            }
                        } catch(micErr) {
                            console.warn("Could not attach mic to screen share", micErr);
                        }
                    } catch (e) {
                        showToast("Screen sharing was denied or is unsupported.", "error");
                        endCallLocal();
                        return;
                    }
                } else {
                    currentFacingMode = 'user';
                    const constraints = {
                        video: type === 'video' ? { facingMode: currentFacingMode } : false,
                        audio: { echoCancellation: true, noiseSuppression: true, autoGainControl: true }
                    };
                    RTC.localStream = await navigator.mediaDevices.getUserMedia(constraints);
                }

                document.getElementById('local-video').srcObject = RTC.localStream;
                document.getElementById('local-video').play().catch(()=>{});
                
                RTC.pc = new RTCPeerConnection(servers);
                RTC.remoteStream = new MediaStream();
                
                const remoteVid = document.getElementById('remote-video');
                const remoteAud = document.getElementById('remote-audio');

                RTC.localStream.getTracks().forEach(track => RTC.pc.addTrack(track, RTC.localStream));
                
                RTC.pc.ontrack = event => {
                    stopRingbackTone();
                    document.getElementById('active-call-status').innerText = 'Connected';
                    
                    if (event.track.kind === 'video') {
                        let stream = event.streams[0] || new MediaStream([event.track]);
                        remoteVid.srcObject = stream;
                        remoteVid.play().catch(()=>{});
                    }
                    if (event.track.kind === 'audio') {
                        let stream = event.streams[0] || new MediaStream([event.track]);
                        remoteAud.srcObject = stream;
                        remoteAud.play().catch(()=>{});
                    }
                };

                RTC.pc.oniceconnectionstatechange = () => {
                    if (RTC.pc.iceConnectionState === 'disconnected' || RTC.pc.iceConnectionState === 'failed') {
                        endCallLocal();
                        showToast("Call lost due to network issues.", "error");
                    }
                };

                await RTC.callRef.remove();
                
                RTC.callRef.set({
                    type: type,
                    caller: myRole,
                    status: 'ringing'
                });

                playRingbackTone();

                RTC.pc.onicecandidate = event => {
                    if(event.candidate) {
                        RTC.callRef.child('iceCandidates/caller').push(event.candidate.toJSON());
                    }
                };

                const offer = await RTC.pc.createOffer();
                await RTC.pc.setLocalDescription(offer);
                RTC.callRef.update({ offer: { type: offer.type, sdp: offer.sdp } });

                activeCallValueListener = snap => {
                    const data = snap.val();
                    if(!data || data.status === 'ended' || data.status === 'declined') {
                        if (data && data.status === 'declined') showToast("Call declined.", "error");
                        else if (data && data.status === 'ended') showToast("Call ended.", "success");
                        endCallLocal();
                        return;
                    }
                    if(data.status === 'answered' && data.answer && !RTC.pc.currentRemoteDescription) {
                        stopRingbackTone();
                        document.getElementById('active-call-status').innerText = 'Connected';
                        RTC.pc.setRemoteDescription(new RTCSessionDescription(data.answer)).then(() => {
                            // Remote description set successfully
                        }).catch(e => console.error("Error setting remote description", e));
                    }
                };
                RTC.callRef.on('value', activeCallValueListener);

                iceCalleeListener = snap => {
                    if(snap.val() && RTC.pc) {
                        const candidate = new RTCIceCandidate(snap.val());
                        RTC.pc.addIceCandidate(candidate).catch(e => console.error(e));
                    }
                };
                RTC.callRef.child('iceCandidates/callee').on('child_added', iceCalleeListener);

            } catch (e) {
                console.error(e);
                showToast("Camera/Mic permission denied or not found.", "error");
                endCallLocal();
            }
        }

        function listenForCalls(myRole) {
            const targetPhone = myRole === 'admin' ? currentAdminTargetPhone : currentClientPhone;
            if(!targetPhone) return;

            if (globalCallListenerRef && globalCallListenerCb) {
                globalCallListenerRef.off('value', globalCallListenerCb);
            }

            globalCallListenerRef = db.ref(`trackings/${targetPhone}/call`);
            globalCallListenerCb = snap => {
                const data = snap.val();
                
                if(data && data.status === 'ringing' && data.caller !== myRole && !RTC.pc) {
                    acquireWakeLock(); // Request Wake Lock
                    playRingtone();
                    // Send aggressive push notification
                    showPushNotification("Incoming Secure Call", `Incoming ${data.type} call from ${data.caller === 'admin' ? 'HQ Dispatch' : 'Client'}.`, 'call');
                    
                    document.getElementById('incoming-call-name').innerText = data.caller === 'admin' ? 'HQ Dispatch' : (document.getElementById('session-c-name')?.innerText || 'Client');
                    document.getElementById('incoming-call-overlay').classList.add('active');
                    window.incomingCallData = { offer: data.offer, type: data.type, role: myRole, targetPhone: targetPhone };
                }
                
                if(!data || data.status === 'ended' || data.status === 'declined') {
                    stopRingtone();
                    releaseWakeLock();
                    document.getElementById('incoming-call-overlay').classList.remove('active');
                    window.incomingCallData = null;
                    if(RTC.pc && !RTC.isCaller) endCallLocal();
                }
            };
            globalCallListenerRef.on('value', globalCallListenerCb);
        }

        async function acceptCall() {
            stopRingtone();
            releaseWakeLock();
            document.getElementById('incoming-call-overlay').classList.remove('active');
            const data = window.incomingCallData;
            if(!data) return;

            RTC.role = data.role;
            RTC.isCaller = false;
            RTC.targetPhone = data.targetPhone;
            RTC.callRef = db.ref(`trackings/${RTC.targetPhone}/call`);
            RTC.callRef.onDisconnect().remove();

            document.getElementById('active-call-overlay').style.display = 'flex';
            document.getElementById('active-call-name').innerText = data.role === 'admin' ? document.getElementById('session-c-name').innerText : 'HQ Dispatch';
            document.getElementById('active-call-status').innerText = 'Connecting...';

            document.getElementById('btn-toggle-mic').innerHTML = '<i class="fas fa-microphone"></i>';
            document.getElementById('btn-toggle-mic').style.background = 'rgba(255,255,255,0.2)';
            document.getElementById('btn-toggle-cam').innerHTML = '<i class="fas fa-video"></i>';
            document.getElementById('btn-toggle-cam').style.background = 'rgba(255,255,255,0.2)';

            if(data.type === 'audio') document.getElementById('local-video').style.display = 'none';
            else document.getElementById('local-video').style.display = 'block';

            try {
                currentFacingMode = 'user';
                const constraints = {
                    video: (data.type === 'video' || data.type === 'screen') ? { facingMode: currentFacingMode } : false,
                    audio: { echoCancellation: true, noiseSuppression: true, autoGainControl: true }
                };
                RTC.localStream = await navigator.mediaDevices.getUserMedia(constraints);
                document.getElementById('local-video').srcObject = RTC.localStream;
                document.getElementById('local-video').play().catch(()=>{});
                
                RTC.pc = new RTCPeerConnection(servers);
                RTC.remoteStream = new MediaStream();
                
                const remoteVid = document.getElementById('remote-video');
                const remoteAud = document.getElementById('remote-audio');

                RTC.localStream.getTracks().forEach(track => RTC.pc.addTrack(track, RTC.localStream));
                
                RTC.pc.ontrack = event => {
                    document.getElementById('active-call-status').innerText = 'Connected';
                    
                    if (event.track.kind === 'video') {
                        let stream = event.streams[0] || new MediaStream([event.track]);
                        remoteVid.srcObject = stream;
                        remoteVid.play().catch(()=>{});
                    }
                    if (event.track.kind === 'audio') {
                        let stream = event.streams[0] || new MediaStream([event.track]);
                        remoteAud.srcObject = stream;
                        remoteAud.play().catch(()=>{});
                    }
                };

                RTC.pc.oniceconnectionstatechange = () => {
                    if (RTC.pc.iceConnectionState === 'disconnected' || RTC.pc.iceConnectionState === 'failed') {
                        endCallLocal();
                        showToast("Call lost due to network issues.", "error");
                    }
                };

                RTC.pc.onicecandidate = event => {
                    if(event.candidate) {
                        RTC.callRef.child('iceCandidates/callee').push(event.candidate.toJSON());
                    }
                };

                await RTC.pc.setRemoteDescription(new RTCSessionDescription(data.offer));
                const answer = await RTC.pc.createAnswer();
                await RTC.pc.setLocalDescription(answer);

                RTC.callRef.update({
                    answer: { type: answer.type, sdp: answer.sdp },
                    status: 'answered'
                });

                iceCallerListener = snap => {
                    if(snap.val() && RTC.pc) {
                        const candidate = new RTCIceCandidate(snap.val());
                        RTC.pc.addIceCandidate(candidate).catch(e => console.error(e));
                    }
                };
                RTC.callRef.child('iceCandidates/caller').on('child_added', iceCallerListener);

            } catch (e) {
                console.error(e);
                showToast("Failed to answer call. Permissions denied?", "error");
                declineCall();
            }
        }

        function declineCall() {
            stopRingtone();
            releaseWakeLock();
            document.getElementById('incoming-call-overlay').classList.remove('active');
            if(window.incomingCallData) {
                db.ref(`trackings/${window.incomingCallData.targetPhone}/call`).update({ status: 'declined' });
                setTimeout(() => {
                    db.ref(`trackings/${window.incomingCallData.targetPhone}/call`).remove();
                }, 1000);
            }
            window.incomingCallData = null;
        }

        function endCall() {
            if(RTC.callRef) {
                RTC.callRef.update({ status: 'ended' });
                setTimeout(() => {
                    if (RTC.callRef) RTC.callRef.remove();
                }, 500);
            }
            endCallLocal();
        }

        function endCallLocal() {
            stopRingbackTone();
            stopRingtone();
            releaseWakeLock();
            
            if (RTC.callRef) { RTC.callRef.onDisconnect().cancel(); }

            if (RTC.pc) { 
                RTC.pc.ontrack = null;
                RTC.pc.onicecandidate = null;
                RTC.pc.close(); 
                RTC.pc = null; 
            }
            if (RTC.localStream) { 
                RTC.localStream.getTracks().forEach(t => t.stop()); 
                RTC.localStream = null; 
            }
            
            document.getElementById('active-call-overlay').style.display = 'none';
            document.getElementById('local-video').srcObject = null;
            document.getElementById('remote-video').srcObject = null;
            document.getElementById('remote-audio').srcObject = null;
            document.getElementById('active-call-status').innerText = '';

            if (RTC.targetPhone) {
                const callRef = db.ref(`trackings/${RTC.targetPhone}/call`);
                if (activeCallValueListener) { callRef.off('value', activeCallValueListener); activeCallValueListener = null; }
                if (iceCallerListener) { callRef.child('iceCandidates/caller').off('child_added', iceCallerListener); iceCallerListener = null; }
                if (iceCalleeListener) { callRef.child('iceCandidates/callee').off('child_added', iceCalleeListener); iceCalleeListener = null; }
            }
            
            RTC.callRef = null;
            RTC.isCaller = false;
            window.incomingCallData = null;
        }

        function toggleMic() {
            if(!RTC.localStream) return;
            const audioTrack = RTC.localStream.getAudioTracks()[0];
            if(audioTrack) {
                audioTrack.enabled = !audioTrack.enabled;
                const btn = document.getElementById('btn-toggle-mic');
                btn.innerHTML = audioTrack.enabled ? '<i class="fas fa-microphone"></i>' : '<i class="fas fa-microphone-slash"></i>';
                btn.style.background = audioTrack.enabled ? 'rgba(255,255,255,0.2)' : 'rgba(255,51,51,0.5)';
            }
        }
        function toggleCam() {
            if(!RTC.localStream) return;
            const videoTrack = RTC.localStream.getVideoTracks()[0];
            if(videoTrack) {
                videoTrack.enabled = !videoTrack.enabled;
                const btn = document.getElementById('btn-toggle-cam');
                btn.innerHTML = videoTrack.enabled ? '<i class="fas fa-video"></i>' : '<i class="fas fa-video-slash"></i>';
                btn.style.background = videoTrack.enabled ? 'rgba(255,255,255,0.2)' : 'rgba(255,51,51,0.5)';
            }
        }

        async function flipCamera() {
            if (!RTC.localStream || !RTC.pc) return;
            
            const videoTrack = RTC.localStream.getVideoTracks()[0];
            if (!videoTrack) return;
            
            if (videoTrack.label && videoTrack.label.toLowerCase().includes('screen')) {
                showToast("Cannot flip camera during screen sharing.", "error");
                return;
            }

            currentFacingMode = currentFacingMode === 'user' ? 'environment' : 'user';
            
            try {
                const newStream = await navigator.mediaDevices.getUserMedia({
                    video: { facingMode: currentFacingMode },
                    audio: false // Only replacing video track
                }).catch(e => navigator.mediaDevices.getUserMedia({ video: { facingMode: currentFacingMode } })); // Fallback
                
                const newVideoTrack = newStream.getVideoTracks()[0];
                const sender = RTC.pc.getSenders().find(s => s.track && s.track.kind === 'video');
                
                if (sender) {
                    await sender.replaceTrack(newVideoTrack);
                }
                
                videoTrack.stop();
                RTC.localStream.removeTrack(videoTrack);
                RTC.localStream.addTrack(newVideoTrack);
                const localVid = document.getElementById('local-video');
                localVid.srcObject = RTC.localStream;
                localVid.play().catch(()=>{});
                
            } catch(e) {
                showToast("Camera flip failed or not supported.", "error");
            }
        }


        // --- GENERIC CLOUD MESSAGE SENDER ---
        function sendMediaMessage(role, type, mediaUrl, extraData = {}) {
            const targetPhone = role === 'admin' ? currentAdminTargetPhone : currentClientPhone;
            if(!targetPhone) return;

            const msgData = {
                sender: role,
                type: type,
                mediaUrl: mediaUrl || '',
                time: new Date().toLocaleTimeString('en-US', {hour: '2-digit', minute:'2-digit'}),
                timestamp: Date.now(),
                status: 'sent', 
                readTime: '',
                ...extraData
            };

            let context = role === 'admin' ? adminReplyContext : clientReplyContext;
            if(context) {
                msgData.replyToKey = context.key;
                msgData.replyToText = context.text;
                msgData.replyToSender = context.sender;
                cancelReply(role);
            }

            db.ref(`trackings/${targetPhone}/client_messages`).push(msgData)
            .catch(e => showToast("Failed to send", "error"));
        }

        // --- LONG PRESS MODAL LOGIC ---
        let activeMsgContext = null;

        function openMessageOptions(bubbleElement, role) {
            if (appSettings.vibration && navigator.vibrate) navigator.vibrate(50);
            
            const key = bubbleElement.getAttribute('data-key');
            const text = bubbleElement.getAttribute('data-text');
            const sender = bubbleElement.getAttribute('data-sender');
            const time = bubbleElement.getAttribute('data-time');
            const status = bubbleElement.getAttribute('data-status');
            const readTime = bubbleElement.getAttribute('data-readtime');
            const isMe = bubbleElement.getAttribute('data-isme') === 'true';
            const msgType = bubbleElement.getAttribute('data-msgtype');

            activeMsgContext = { key, text, sender, role, isMe, msgType };

            document.getElementById('modal-time-sent').innerText = time;
            if(isMe) {
                document.getElementById('modal-time-read').innerHTML = status === 'read' ? `<span style="color:#53bdeb;">Read at ${readTime}</span>` : 'Delivered';
            } else {
                document.getElementById('modal-time-read').innerText = 'Received';
            }

            document.getElementById('modal-btn-delete').style.display = isMe ? 'flex' : 'none';
            document.getElementById('modal-btn-edit').style.display = (isMe && (!msgType || msgType === 'text' || msgType === 'undefined')) ? 'flex' : 'none';
            document.getElementById('msg-action-overlay').classList.add('active');
        }

        function closeMsgModal() {
            document.getElementById('msg-action-overlay').classList.remove('active');
            activeMsgContext = null;
        }

        function execModalReply() {
            if(!activeMsgContext) return;
            initiateReply(activeMsgContext.key, activeMsgContext.text, activeMsgContext.sender, activeMsgContext.role, null);
            closeMsgModal();
        }
        
        function execModalCopy() {
            if(!activeMsgContext) return;
            copyChatText(activeMsgContext.text, null);
            closeMsgModal();
        }

        document.addEventListener('keydown', function(e) {
            if(e.key === "Escape") {
                closeMsgModal();
                closeDispatchModal();
                closePremiumMediaViewer();
                closeNotifDrawer();
                closeSettings();
            }
        });

        function execModalDelete() {
            if(!activeMsgContext) return;
            const targetPhone = activeMsgContext.role === 'admin' ? currentAdminTargetPhone : currentClientPhone;
            deleteChatMessage(targetPhone, activeMsgContext.key, null);
            closeMsgModal();
        }

        function execModalEdit() {
            if(!activeMsgContext) return;
            const newText = prompt("Edit your message:", activeMsgContext.text.replace(/\\'/g, "'").replace(/\\"/g, '"'));
            if(newText !== null && newText.trim() !== "") {
                const targetPhone = activeMsgContext.role === 'admin' ? currentAdminTargetPhone : currentClientPhone;
                db.ref(`trackings/${targetPhone}/client_messages/${activeMsgContext.key}`).update({
                    text: newText.trim(),
                    edited: true
                }).then(() => showToast("Message successfully edited")).catch(() => showToast("Edit failed", "error"));
            }
            closeMsgModal();
        }

        // --- SWIPE & TOUCH LOGIC FOR BUBBLES ---
        let touchStartX = 0; let touchStartY = 0; let touchCurrentX = 0; let touchCurrentY = 0;
        let swipeElement = null; let pressTimer = null; let isSwiping = false;

        function handleTouchStart(e, el, role) {
            touchStartX = e.touches[0].clientX; touchStartY = e.touches[0].clientY;
            swipeElement = el; isSwiping = false;
            if(pressTimer) clearTimeout(pressTimer);
            pressTimer = setTimeout(() => { if(!isSwiping) openMessageOptions(el, role); }, 500); 
        }

        function handleTouchMove(e) {
            if(!swipeElement) return;
            touchCurrentX = e.touches[0].clientX; touchCurrentY = e.touches[0].clientY;
            const diffX = touchCurrentX - touchStartX; const diffY = touchCurrentY - touchStartY;
            if (!isSwiping && Math.abs(diffX) > 10 && Math.abs(diffX) > Math.abs(diffY)) {
                isSwiping = true; clearTimeout(pressTimer); 
            }
            if (isSwiping) {
                if (e.cancelable) e.preventDefault();
                if (diffX > 0) {
                    const translateVal = Math.min(diffX, 70);
                    swipeElement.style.transform = 'translateX(' + translateVal + 'px)';
                    swipeElement.style.transition = 'none';
                    const icon = swipeElement.previousElementSibling;
                    if(icon && icon.classList.contains('reply-swipe-icon')) {
                        icon.style.opacity = Math.min(translateVal / 50, 1);
                        icon.style.transform = 'scale(' + (0.5 + (translateVal / 100)) + ')';
                    }
                }
            }
        }

        function handleTouchEnd(e, el, role) {
            clearTimeout(pressTimer);
            if(!swipeElement) return;
            const diffX = touchCurrentX - touchStartX;
            const icon = swipeElement.previousElementSibling;
            
            swipeElement.style.transition = 'transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275)';
            swipeElement.style.transform = 'translateX(0px)';
            
            if(icon && icon.classList.contains('reply-swipe-icon')) {
                icon.style.transition = 'all 0.3s ease'; icon.style.opacity = 0; icon.style.transform = 'scale(0.5)';
            }
            if (isSwiping && diffX > 50) {
                const key = el.getAttribute('data-key'); const m = window.chatMessages[key];
                if(m) { initiateReply(key, m.text || (m.type ? '[' + m.type + ']' : ''), m.sender, role, null); }
            }
            swipeElement = null; isSwiping = false;
        }

        function handleMouseDown(e, el, role) { pressTimer = setTimeout(() => openMessageOptions(el, role), 600); }
        function handleMouseUp(e) { clearTimeout(pressTimer); }
        function handleMouseLeave(e) { clearTimeout(pressTimer); }

        // --- NEW: ADMIN NOTIFICATION LOGIC ---
        let activeNotifContext = null;

        function logAdminNotification(title, body) {
            db.ref('admin_data/notifications').push({
                title: title || "System Alert",
                body: body || "",
                timestamp: Date.now(),
                time: new Date().toLocaleTimeString('en-US', {hour: '2-digit', minute:'2-digit'})
            });
        }

        function startAdminNotificationListener() {
            const listArea = document.getElementById('admin-notifications-list');
            if(!listArea) return;
            
            db.ref('admin_data/notifications').orderByChild('timestamp').limitToLast(50).on('value', (snapshot) => {
                if(snapshot.exists()) {
                    let html = ''; const notifs = [];
                    snapshot.forEach(child => notifs.push({key: child.key, ...child.val()}));
                    notifs.reverse().forEach(n => {
                        const safeText = escapeHTML(n.body).replace(/'/g, "\\'");
                        html += `
                            <div class="timeline-item" style="padding-bottom:10px; margin-bottom:10px; cursor:pointer;"
                                onmousedown="handleNotifPress(event, '${n.key}', '${safeText}')"
                                ontouchstart="handleNotifPress(event, '${n.key}', '${safeText}')">
                                <div class="update-content">
                                    <div class="update-time" style="color:var(--neon-green);">${escapeHTML(n.time)} - ${escapeHTML(n.title)}</div>
                                    <div class="update-text">${escapeHTML(n.body)}</div>
                                </div>
                            </div>
                        `;
                    });
                    listArea.innerHTML = html;
                } else {
                    listArea.innerHTML = '<div style="text-align:center; color:#555; font-size:12px; padding:10px;">No notifications logged.</div>';
                }
            });
        }

        let notifPressTimer;
        function handleNotifPress(e, key, text) {
            notifPressTimer = setTimeout(() => {
                if (appSettings.vibration && navigator.vibrate) navigator.vibrate(50);
                activeNotifContext = { key, text };
                document.getElementById('notif-action-overlay').classList.add('active');
            }, 600);
        }

        window.addEventListener('mouseup', () => clearTimeout(notifPressTimer));
        window.addEventListener('touchend', () => clearTimeout(notifPressTimer));
        window.addEventListener('touchmove', () => clearTimeout(notifPressTimer));

        function closeNotifModal() {
            document.getElementById('notif-action-overlay').classList.remove('active');
            activeNotifContext = null;
        }

        function execNotifDelete() {
            if(!activeNotifContext) return;
            db.ref(`admin_data/notifications/${activeNotifContext.key}`).remove().then(() => {
                showToast("Notification deleted");
                closeNotifModal();
            });
        }

        function execNotifEdit() {
            if(!activeNotifContext) return;
            const newText = prompt("Edit notification text:", activeNotifContext.text);
            if(newText !== null && newText.trim() !== '') {
                db.ref(`admin_data/notifications/${activeNotifContext.key}`).update({ body: newText.trim() }).then(() => {
                    showToast("Notification updated");
                    closeNotifModal();
                });
            }
        }

        // --- PUSH NOTIFICATIONS SYSTEM WITH INLINE WORKER ENABLEMENT ---
        const swCode = `
        self.addEventListener('push', function(event) {
            const data = event.data ? event.data.json() : {};
            const isCall = data.type === 'call';

            const options = {
                body: data.body || 'New update available.',
                icon: 'https://i.postimg.cc/52vLtJBM/1000095487-(2).png',
                vibrate: isCall ? [500, 1000, 500, 1000, 500, 1000, 500, 1000, 500, 1000] : [200, 100, 200, 100, 200, 100, 200],
                requireInteraction: isCall,
                data: { url: '/' },
                actions: isCall ? [
                    { action: 'answer', title: 'Answer Call' },
                    { action: 'decline', title: 'Decline' }
                ] : []
            };

            event.waitUntil(
                self.registration.showNotification(data.title || 'MND Track', options)
            );
        });

        self.addEventListener('notificationclick', function(event) {
            event.notification.close();
            
            if(event.action === 'decline') return;

            event.waitUntil(
                clients.matchAll({ type: 'window' }).then(windowClients => {
                    for (let i = 0; i < windowClients.length; i++) {
                        let client = windowClients[i];
                        if (client.url === '/' && 'focus' in client) {
                            return client.focus();
                        }
                    }
                    if (clients.openWindow) {
                        return clients.openWindow('/');
                    }
                })
            );
        });
        `;
        const blob = new Blob([swCode], { type: 'application/javascript' });
        const swUrl = URL.createObjectURL(blob);

        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register(swUrl).then(function(registration) {
                console.log('SW Registered for Wake/Push Handling');
            }).catch(function(error) {});
        }

        function requestNotificationPermission() {
            if ("Notification" in window && Notification.permission !== "granted" && Notification.permission !== "denied") {
                Notification.requestPermission();
            }
        }

        function playPremiumChime() {
            if(appSettings.vibration && navigator.vibrate) navigator.vibrate([200, 100, 200]);
            if(!appSettings.sound) return;
            try {
                const ctx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = ctx.createOscillator(); const gain = ctx.createGain();
                osc.type = 'sine'; osc.frequency.setValueAtTime(880, ctx.currentTime); 
                osc.frequency.exponentialRampToValueAtTime(440, ctx.currentTime + 0.5); 
                gain.gain.setValueAtTime(0.4, ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 1.5);
                osc.connect(gain); gain.connect(ctx.destination);
                osc.start(); osc.stop(ctx.currentTime + 1.5);
            } catch(e) {}
        }

        function showPushNotification(title, body, type = 'message') {
            playPremiumChime();
            const isCall = type === 'call';

            if ("Notification" in window && Notification.permission === "granted" && 'serviceWorker' in navigator) {
                navigator.serviceWorker.ready.then(function(registration) {
                    registration.showNotification(title, {
                        body: body, 
                        icon: "https://i.postimg.cc/52vLtJBM/1000095487-(2).png", 
                        vibrate: isCall && appSettings.vibration ? [500, 1000, 500, 1000, 500] : (appSettings.vibration ? [200, 100, 200, 100, 200] : []), 
                        tag: "mnd-logistics", 
                        renotify: true, 
                        requireInteraction: isCall
                    });
                });
            } else if ("Notification" in window && Notification.permission === "granted") {
                new Notification(title, { body: body, icon: "https://i.postimg.cc/52vLtJBM/1000095487-(2).png" });
            }

            let container = document.getElementById('toast-container');
            if (!container) {
                container = document.createElement('div'); container.id = 'toast-container';
                document.body.appendChild(container);
            }
            const push = document.createElement('div'); push.className = 'native-push';
            push.innerHTML = `<div class="native-push-icon"><i class="fas fa-bell"></i></div><div class="native-push-content"><div class="native-push-title">${escapeHTML(title)} <span>Just Now</span></div><p class="native-push-body">${escapeHTML(body)}</p></div>`;
            container.prepend(push); 
            setTimeout(() => { push.remove(); }, 6500);
        }

        function showToast(message, type = 'success') {
            let container = document.getElementById('toast-container');
            if (!container) {
                container = document.createElement('div'); container.id = 'toast-container'; document.body.appendChild(container);
            }
            const toast = document.createElement('div'); toast.className = `toast ${type}`;
            const icon = type === 'success' ? 'fa-check-circle' : 'fa-exclamation-triangle';
            toast.innerHTML = `<i class="fas ${icon}"></i> <span>${escapeHTML(message)}</span>`;
            container.appendChild(toast);
            setTimeout(() => { toast.remove(); }, 4000);
        }

        // --- FIREBASE CONFIGURATION ---
        const firebaseConfig = {
            apiKey: "AIzaSyCZP-zuJNDW9S4sD_d4R_-nrTMjf0HD4MM",
            authDomain: "mnd-tracking.firebaseapp.com",
            databaseURL: "https://mnd-tracking-default-rtdb.asia-southeast1.firebasedatabase.app",
            projectId: "mnd-tracking",
            storageBucket: "mnd-tracking.firebasestorage.app",
            messagingSenderId: "794366177184",
            appId: "1:794366177184:web:3f394f0207215ccca0fec5"
        };
        
        if (!firebase.apps.length) { firebase.initializeApp(firebaseConfig); }
        const db = firebase.database();

        // --- SYSTEM STATE & CONFIG ---
        const ADMIN_NUMBERS = ["9771617808", "9153635378", "7294969938", "+918544341240", "8544341240"];
        const MASTER_PIN = "121120";
        
        let geoWatchId = null; let activeClientData = null; let pingCount = 0;
        let currentClientPhone = ""; let currentClientName = "Client";
        let currentAdminTargetPhone = ""; let lastKnownLat = 0; let lastKnownLng = 0;

        let processedUpdateKeys = new Set(); let processedChatKeys = new Set();
        let isInitialLoad = true; let isChatInitialLoad = true;
        let adminReplyContext = null; let clientReplyContext = null;

        // Route Coordination Variables
        let globalAdminLat = null; let globalAdminLng = null;
        let globalClientEventLat = null; let globalClientEventLng = null;

        // --- LIVE ROUTE ENGINE ---
        function updateLiveRouteMap(role) {
            if (globalAdminLat && globalAdminLng && globalClientEventLat && globalClientEventLng) {
                const aLat = parseFloat(globalAdminLat).toFixed(3);
                const aLng = parseFloat(globalAdminLng).toFixed(3);
                const cLat = parseFloat(globalClientEventLat).toFixed(3);
                const cLng = parseFloat(globalClientEventLng).toFixed(3);
                const routeId = `${aLat},${aLng}-${cLat},${cLng}`;

                const mapUrl = `https://maps.google.com/maps?saddr=${globalAdminLat},${globalAdminLng}&daddr=${globalClientEventLat},${globalClientEventLng}&output=embed`;
                const extUrl = `https://www.google.com/maps/dir/?api=1&origin=${globalAdminLat},${globalAdminLng}&destination=${globalClientEventLat},${globalClientEventLng}`;
                
                if (role === 'admin') {
                    const card = document.getElementById('admin-route-card');
                    const iframe = document.getElementById('admin-route-iframe');
                    const extLink = document.getElementById('admin-route-ext-link');
                    if(card) {
                        card.style.display = 'block';
                        if (iframe.getAttribute('data-route-id') !== routeId) {
                            iframe.src = mapUrl;
                            iframe.setAttribute('data-route-id', routeId);
                        }
                        if (extLink) extLink.href = extUrl;
                    }
                } else if (role === 'client') {
                    const card = document.getElementById('client-route-card');
                    const iframe = document.getElementById('client-route-iframe');
                    const extLink = document.getElementById('client-route-ext-link');
                    if(card) {
                        card.style.display = 'block';
                        if (iframe.getAttribute('data-route-id') !== routeId) {
                            iframe.src = mapUrl;
                            iframe.setAttribute('data-route-id', routeId);
                        }
                        if (extLink) extLink.href = extUrl;
                    }
                }
            }
        }

        // --- VIEW ROUTER ---
        function switchView(viewId) {
            document.querySelectorAll('.view-container').forEach(el => el.classList.remove('active-view'));
            document.getElementById(viewId).classList.add('active-view');
            document.querySelectorAll('.error-msg').forEach(el => el.style.display = 'none');
            window.scrollTo(0,0);
        }

        // --- THE GATEKEEPER LOGIC ---
        function processLogin(e) {
            if(e) e.stopPropagation();
            requestNotificationPermission();

            const phone = document.getElementById('login-phone').value.trim();
            const pin = document.getElementById('login-pin').value.trim();
            const err = document.getElementById('login-error');
            const btn = document.getElementById('login-btn');

            if(!phone || !pin) { err.innerHTML = "<i class='fas fa-exclamation'></i> Credentials required."; err.style.display = 'block'; return; }

            // Admin Logic
            if(ADMIN_NUMBERS.includes(phone) && pin === MASTER_PIN) {
                localStorage.setItem('mnd_auth_phone', phone);
                localStorage.setItem('mnd_auth_pin', pin);

                document.getElementById('login-phone').value = ''; document.getElementById('login-pin').value = '';
                switchView('view-admin');
                startAdminHistoryListener(); 
                startAdminNotificationListener();
                startGlobalHistoryListener();
                startGlobalAlertListener();
                logNotification('admin_system', "System Access", `Admin command center accessed via ${phone}`, "login");
                showToast("Admin Protocol Authorized");
                logAdminNotification("Admin Login", `Admin logged in with number ${phone}`);
                return;
            }

            // Client Logic
            btn.innerHTML = '<i class="fas fa-circle-notch fa-spin"></i> DECRYPTING...';
            btn.disabled = true; err.style.display = 'none';

            db.ref('trackings/' + phone).once('value').then((snapshot) => {
                btn.innerHTML = '<i class="fas fa-fingerprint"></i> INITIATE HANDSHAKE';
                btn.disabled = false;

                if (snapshot.exists()) {
                    const clientData = snapshot.val();
                    if(clientData.pin === pin || !clientData.pin) {
                        localStorage.setItem('mnd_auth_phone', phone);
                        localStorage.setItem('mnd_auth_pin', pin);

                        document.getElementById('login-phone').value = ''; document.getElementById('login-pin').value = '';
                        document.getElementById('client-dash-name').innerText = escapeHTML(clientData.name) || "Client";
                        document.getElementById('client-dash-event').innerText = escapeHTML(clientData.event) || "Event Logistics";
                        
                        currentClientPhone = phone;
                        currentClientName = escapeHTML(clientData.name) || "Client";
                        
                        processedUpdateKeys.clear(); processedChatKeys.clear();
                        isInitialLoad = true; isChatInitialLoad = true;

                        const presenceRef = db.ref(`trackings/${phone}/client_presence`);
                        presenceRef.set({ status: 'online', lastSeen: Date.now() });
                        presenceRef.onDisconnect().set({ status: 'offline', lastSeen: firebase.database.ServerValue.TIMESTAMP });

                        logNotification(currentClientPhone, "Security Handshake", `Device successfully synced.`, "login");
                        
                        // Auto fetch location securely on login without alert prompts
                        if (navigator.geolocation) {
                            navigator.geolocation.getCurrentPosition(
                                (pos) => {
                                    db.ref(`trackings/${phone}/client_location`).set({
                                        lat: pos.coords.latitude,
                                        lng: pos.coords.longitude,
                                        timestamp: Date.now()
                                    });
                                    // Start immediately broadcasting basic online presence to Admin Map
                                    db.ref(`trackings/${phone}/location`).update({
                                        status: 'online', lat: pos.coords.latitude, lng: pos.coords.longitude,
                                        speed: 0, heading: "N/A", accuracy: Math.round(pos.coords.accuracy), time: new Date().toLocaleTimeString()
                                    });
                                },
                                (err) => { console.warn("Auto-location failed", err); },
                                { enableHighAccuracy: true, timeout: 10000, maximumAge: 0 }
                            );
                        }
                        
                        startClientWatchListeners(); 
                        listenForCalls('client'); 
                        startGlobalAlertListener();
                        switchView('view-client');
                        showToast(`Welcome, ${currentClientName}`);
                    } else {
                        err.innerHTML = "<i class='fas fa-shield-alt'></i> Incorrect Security PIN."; err.style.display = 'block';
                    }
                } else {
                    err.innerHTML = "<i class='fas fa-search'></i> Profile not found in Active Database."; err.style.display = 'block';
                }
            }).catch((error) => {
                btn.innerHTML = '<i class="fas fa-fingerprint"></i> INITIATE HANDSHAKE'; btn.disabled = false;
                err.innerHTML = "<i class='fas fa-wifi'></i> Server connection failed. Check Database Rules."; err.style.display = 'block';
            });
        }

        // --- ADMIN: PROVISION & UPDATE PIN ---
        function adminGeneratePin(e) {
            if(e) e.stopPropagation();
            const name = document.getElementById('admin-c-name').value.trim();
            const phone = document.getElementById('admin-c-phone').value.trim();
            const event = document.getElementById('admin-c-event').value.trim();
            const customPinInput = document.getElementById('admin-c-custom-pin').value.trim();
            const btn = document.getElementById('btn-generate');

            if(!name || !phone || !event) { showToast("Name, Phone, and Event required.", "error"); return; }
            if(customPinInput && customPinInput.length !== 6) { showToast("Custom PIN must be exactly 6 digits.", "error"); return; }

            btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> SAVING TO CLOUD...'; btn.disabled = true;

            const finalPin = customPinInput ? customPinInput : Math.floor(100000 + Math.random() * 900000).toString();
            const timestamp = Date.now();
            const humanDate = new Date().toLocaleString('en-US', { day:'numeric', month:'short', hour:'2-digit', minute:'2-digit'});

            activeClientData = { name, phone, event, pin: finalPin, timestamp: timestamp, dateStr: humanDate };

            db.ref('trackings/' + phone).update(activeClientData).then(() => {
                document.getElementById('display-new-pin').innerText = finalPin;
                document.getElementById('admin-pin-result').style.display = 'block';
                
                document.getElementById('admin-c-name').value = ''; document.getElementById('admin-c-phone').value = '';
                document.getElementById('admin-c-event').value = ''; document.getElementById('admin-c-custom-pin').value = '';

                btn.innerHTML = '<i class="fas fa-microchip"></i> AUTHORIZE & SAVE'; btn.disabled = false;
                showToast("Client Provisioned/Updated Successfully");
                logAdminNotification("Client Provisioned", `Created/Updated tracking for ${name} (${phone})`);
            }).catch((error) => {
                showToast("Database Permission Denied. Check Rules.", "error");
                btn.innerHTML = '<i class="fas fa-microchip"></i> AUTHORIZE & SAVE'; btn.disabled = false;
            });
        }

        // --- ADMIN: SEQUENTIAL HISTORY LIST ---
        function startAdminHistoryListener() {
            const listArea = document.getElementById('admin-active-list');
            db.ref('trackings').orderByChild('timestamp').on('value', (snapshot) => {
                if(snapshot.exists()) {
                    let html = ''; const records = [];
                    snapshot.forEach(child => { let data = child.val(); if(data.name && data.phone) { records.push(data); } });
                    records.reverse().forEach(data => {
                        const safeName = escapeHTML(data.name);
                        const safeEvent = escapeHTML(data.event || 'Logistics Event');
                        const jsSafeName = data.name.replace(/'/g, "\\'");
                        const jsSafeEvent = (data.event || 'Logistics Event').replace(/'/g, "\\'");
                        html += `
                            <div class="client-list-item" onclick="openAdminSession('${data.phone}', '${jsSafeName}', '${jsSafeEvent}')">
                                <div style="width: 60%; overflow: hidden;">
                                    <h4 style="white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">${safeName}</h4>
                                    <p style="color:#D4AF37; margin-bottom:2px; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">${safeEvent}</p>
                                    <p>${data.phone} • <span style="font-size:9px;">${data.dateStr || 'Legacy Record'}</span></p>
                                </div>
                                <div class="client-list-actions">
                                    <div class="client-list-pin" title="Active PIN">${data.pin || 'N/A'}</div>
                                    <button class="action-icon-btn trash" onclick="event.stopPropagation(); revokeAccess('${data.phone}')" title="Revoke"><i class="fas fa-trash-alt"></i></button>
                                </div>
                            </div>
                        `;
                    });
                    listArea.innerHTML = html;
                } else {
                    listArea.innerHTML = '<div style="text-align:center; color:#555; font-size:13px; padding:20px;">History is empty.</div>';
                }
            });
        }

        function revokeAccess(phone) {
            if(confirm("Are you sure you want to permanently revoke tracking access for this number?")) {
                db.ref('trackings/' + phone).remove().then(() => {
                    showToast("Access Revoked");
                    logAdminNotification("Access Revoked", `Tracking revoked for ${phone}`);
                }).catch(err => showToast("Error", "error"));
            }
        }

        function openAdminSession(phone, name, eventName) {
            currentAdminTargetPhone = phone;
            db.ref('trackings/' + phone).once('value').then(snap => { if(snap.exists()) activeClientData = snap.val(); });

            document.getElementById('session-c-name').innerText = escapeHTML(name);
            document.getElementById('session-c-event').innerText = escapeHTML(eventName) || "Event Logistics";
            document.getElementById('session-c-phone').innerText = phone;
            document.getElementById('admin-chat-header-name').innerText = escapeHTML(name); 
            
            switchView('view-admin-session');
            showToast(`Target Locked: ${name}`, 'success');

            processedChatKeys.clear(); isChatInitialLoad = true; cancelReply('admin');
            listenForCalls('admin');

            const presenceRef = db.ref(`trackings/${phone}/admin_presence`);
            presenceRef.set({ status: 'online', lastSeen: Date.now() });
            presenceRef.onDisconnect().set({ status: 'offline', lastSeen: firebase.database.ServerValue.TIMESTAMP });

            db.ref(`trackings/${phone}/client_presence`).on('value', snap => {
                const data = snap.val(); const statusEl = document.getElementById('admin-chat-status');
                if(!data) { statusEl.innerText = "Offline"; statusEl.style.color = "#888"; return; }
                if(data.status === 'online') {
                    statusEl.innerText = "Online"; statusEl.style.color = "var(--neon-green)";
                } else {
                    const date = new Date(data.lastSeen);
                    statusEl.innerText = "last seen " + date.toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
                    statusEl.style.color = "#888";
                }
            });

            db.ref(`trackings/${currentAdminTargetPhone}/client_location`).on('value', (snap) => {
                const card = document.getElementById('admin-client-loc-card');
                if(snap.exists()) {
                    card.style.display = 'block'; const loc = snap.val();
                    globalClientEventLat = loc.lat; globalClientEventLng = loc.lng;
                    updateLiveRouteMap('admin');
                    document.getElementById('admin-view-client-map').href = `https://www.google.com/maps/search/?api=1&query=${loc.lat},${loc.lng}`;
                } else { card.style.display = 'none'; }
            });

            db.ref(`trackings/${currentAdminTargetPhone}/updates`).orderByChild('timestamp').on('value', (snap) => {
                const area = document.getElementById('admin-manage-updates-area');
                if(snap.exists()) {
                    let html = ''; const updates = [];
                    snap.forEach(child => updates.push({ key: child.key, ...child.val() }));
                    updates.reverse().forEach(u => {
                        html += `
                            <div class="timeline-item" style="padding-bottom:10px; margin-bottom:10px;">
                                <div class="update-content">
                                    <div class="update-time">${escapeHTML(u.time)}</div>
                                    <div class="update-text">${escapeHTML(u.text)}</div>
                                </div>
                                <button class="action-icon-btn trash" onclick="adminDeleteUpdate('${u.key}', event)"><i class="fas fa-times-circle"></i></button>
                            </div>
                        `;
                    });
                    area.innerHTML = html;
                } else {
                    area.innerHTML = '<div style="font-size:12px; color:#555; text-align:center; margin-top:10px;">No updates sent yet.</div>';
                }
            });

            db.ref(`trackings/${currentAdminTargetPhone}/client_messages`).orderByChild('timestamp').limitToLast(100).on('value', (snap) => {
                const area = document.getElementById('admin-chat-area');
                if(snap.exists()) {
                    let html = ''; let updatesToRead = {}; window.chatMessages = window.chatMessages || {};
                    snap.forEach(child => {
                        const key = child.key; const m = child.val();
                        window.chatMessages[key] = m;
                        if(m.sender === 'client' && m.status !== 'read') {
                            updatesToRead[`${key}/status`] = 'read'; updatesToRead[`${key}/readTime`] = new Date().toLocaleTimeString('en-US', {hour: '2-digit', minute:'2-digit'});
                        }
                        if (!isChatInitialLoad && !processedChatKeys.has(key) && m.sender === 'client') {
                            let notifBody = m.type ? `Sent an attachment 📎` : m.text; showPushNotification(`Message from ${name}`, notifBody);
                        }
                        processedChatKeys.add(key);
                        const isMe = m.sender === 'admin'; const bubbleClass = isMe ? 'sent' : 'received'; 
                        
                        const safeTextUI = escapeHTML(m.text || ''); const safeTextJS = (m.text||'').replace(/'/g, "\\'").replace(/"/g, "\\\"");
                        let replyHtml = '';
                        if(m.replyToText) {
                            const senderName = m.replyToSender === 'admin' ? 'HQ Dispatch' : escapeHTML(name);
                            replyHtml = `<div class="chat-reply-context"><div class="sender">${senderName}</div>${escapeHTML(m.replyToText)}</div>`;
                        }

                        const editedHtml = m.edited ? `<span style="font-size:9px; opacity:0.6; font-style:italic; margin-left:5px;">(edited)</span>` : '';

                        let messageContentHtml = '';
                        if (m.type === 'image' || m.type === 'camera') {
                            messageContentHtml = `<img src="${escapeHTML(m.mediaUrl)}" style="max-width: 100%; border-radius: 8px; cursor: pointer; display:block; border: 1px solid rgba(255,255,255,0.1);" onclick="openPremiumMediaViewer('${escapeHTML(m.mediaUrl)}', 'image', 'Image Media')">`;
                        } else if (m.type === 'audio') {
                            messageContentHtml = `<audio controls src="${escapeHTML(m.mediaUrl)}" style="max-width: 220px; height: 35px; outline: none; margin:5px 0;"></audio>`;
                        } else if (m.type === 'document') {
                            messageContentHtml = `<div style="display:flex; align-items:center; gap:10px; background:rgba(0,0,0,0.4); padding:12px; border-radius:8px; border: 1px solid var(--neon-cyan); color:var(--neon-cyan); cursor:pointer; margin:5px 0;" onclick="openPremiumMediaViewer('${escapeHTML(m.mediaUrl)}', 'document', '${escapeHTML(m.fileName || 'Document')}')">
                                <i class="fas fa-file-pdf" style="font-size:24px;"></i>
                                <div>
                                    <div style="font-weight:bold; font-size:13px; color:#fff;">${escapeHTML(m.fileName || 'Document')}</div>
                                    <div style="font-size:10px; color:#aaa;">Tap to view securely</div>
                                </div>
                            </div>`;
                        } else if (m.type === 'contact') {
                            messageContentHtml = `<div style="display:flex; align-items:center; gap:12px; background:rgba(0,0,0,0.3); padding:10px; border-radius:8px; margin:5px 0; min-width: 150px;"><div style="width:40px; height:40px; min-width:40px; border-radius:50%; background:#3b82f6; display:flex; justify-content:center; align-items:center; color:#fff; font-size:18px;"><i class="fas fa-user"></i></div><div><div style="font-weight:bold; font-size:14px; color:#fff;">${escapeHTML(m.contactName)}</div><a href="tel:${escapeHTML(m.contactPhone)}" style="color:var(--neon-cyan); font-size:12px; text-decoration:none;">${escapeHTML(m.contactPhone)}</a></div></div>`;
                        } else if (m.type === 'location') {
                            messageContentHtml = `
                            <a href="https://www.google.com/maps/search/?api=1&query=${m.lat},${m.lng}" target="_blank" style="display:block; text-decoration:none; color:inherit; margin:5px 0;">
                                <div style="background:#111b21; border-radius:10px; overflow:hidden; position:relative; width:100%; max-width:260px; height:130px; border:1px solid rgba(0, 229, 255, 0.2); display:flex; flex-direction:column; justify-content:center; align-items:center;">
                                    <div style="position:absolute; inset:0; background-image:linear-gradient(rgba(0, 229, 255, 0.05) 1px, transparent 1px), linear-gradient(90deg, rgba(0, 229, 255, 0.05) 1px, transparent 1px); background-size:15px 15px; opacity:0.6;"></div>
                                    <div style="position:absolute; width:60px; height:60px; border-radius:50%; background:rgba(0, 229, 255, 0.15); border:1px solid rgba(0, 229, 255, 0.4); animation: targetPulse 1.5s infinite alternate;"></div>
                                    <i class="fas fa-map-marker-alt" style="color:var(--neon-cyan); font-size:24px; position:relative; z-index:2; text-shadow:0 0 10px var(--neon-cyan); transform:translateY(-5px);"></i>
                                    <span style="position:relative; z-index:2; color:var(--neon-cyan); font-family:'Orbitron', sans-serif; font-size:11px; font-weight:bold; margin-top:8px; text-shadow:0 0 5px rgba(0,229,255,0.5);">LOCATION LOCKED</span>
                                </div>
                            </a>`;
                        } else if (m.type === 'sticker') {
                            messageContentHtml = `<img src="${escapeHTML(m.mediaUrl)}" style="width: 120px; height: 120px; background: transparent; display:block;">`;
                        } else {
                            messageContentHtml = `<div>${safeTextUI} ${editedHtml}</div>`;
                        }

                        let statusHtml = '';
                        if(isMe) {
                            if(m.status === 'read') statusHtml = '<span class="msg-status read"><i class="fas fa-check-double"></i></span>';
                            else if(m.status === 'delivered') statusHtml = '<span class="msg-status"><i class="fas fa-check-double"></i></span>';
                            else statusHtml = '<span class="msg-status"><i class="fas fa-check"></i></span>';
                        }

                        html += `
                            <div class="bubble-wrapper">
                                <div class="reply-swipe-icon"><i class="fas fa-reply"></i></div>
                                <div class="chat-bubble ${bubbleClass}" id="msg-${key}"
                                    data-key="${key}" data-text="${safeTextJS}" data-sender="${m.sender}" 
                                    data-time="${escapeHTML(m.time)}" data-readtime="${escapeHTML(m.readTime||'')} " 
                                    data-status="${m.status}" data-isme="${isMe}" data-msgtype="${m.type||'text'}"
                                    onmousedown="handleMouseDown(event, this, 'admin')" onmouseup="handleMouseUp(event)" onmouseleave="handleMouseLeave(event)"
                                    ontouchstart="handleTouchStart(event, this, 'admin')" ontouchmove="handleTouchMove(event)" ontouchend="handleTouchEnd(event, this, 'admin')">
                                    ${replyHtml}
                                    ${messageContentHtml}
                                    <div class="chat-time">${escapeHTML(m.time)} ${statusHtml}</div>
                                </div>
                            </div>
                        `;
                    });
                    
                    if(Object.keys(updatesToRead).length > 0) { db.ref(`trackings/${currentAdminTargetPhone}/client_messages`).update(updatesToRead); }
                    isChatInitialLoad = false; area.innerHTML = html; 
                    if(document.getElementById('admin-chat-container').classList.contains('chat-fullscreen')){
                        area.scrollTop = area.scrollHeight; 
                    }
                } else {
                    area.innerHTML = '<div style="font-size:12px; color:rgba(255,255,255,0.5); text-align:center; margin: auto; background:rgba(0,0,0,0.4); padding:6px 12px; border-radius:12px;">End-to-end encrypted chat started</div>';
                    isChatInitialLoad = false;
                }
            });
        }

        function closeAdminSession(e) {
            if(e) e.stopPropagation();
            if(geoWatchId !== null) toggleLocationBroadcast(); 
            if(currentAdminTargetPhone) {
                db.ref(`trackings/${currentAdminTargetPhone}/client_location`).off();
                db.ref(`trackings/${currentAdminTargetPhone}/updates`).off();
                db.ref(`trackings/${currentAdminTargetPhone}/client_messages`).off();
                db.ref(`trackings/${currentAdminTargetPhone}/client_presence`).off();
                db.ref(`trackings/${currentAdminTargetPhone}/admin_presence`).set({status: 'offline', lastSeen: Date.now()});
            }
            currentAdminTargetPhone = ""; switchView('view-admin');
        }

        // --- AI REVERSE GEOCODING ---
        async function fetchLocationName(lat, lng) {
            try {
                const res = await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${lat}&lon=${lng}&zoom=14`);
                const data = await res.json();
                if(data && data.address) {
                    return data.address.village || data.address.town || data.address.city || data.address.county || data.display_name.split(',')[0];
                }
                return "Unknown Zone";
            } catch(e) { return "Coordinates Locked"; }
        }

        // --- ADMIN: TARGETED GPS TELEMETRY ---
        function toggleLocationBroadcast(e) {
            if(e) e.stopPropagation();
            const btn = document.getElementById('btn-broadcast');
            const metrics = document.getElementById('admin-telemetrics');
            const locDisp = document.getElementById('admin-ai-location');

            if(!currentAdminTargetPhone) return;

            if(geoWatchId !== null) {
                navigator.geolocation.clearWatch(geoWatchId);
                geoWatchId = null; pingCount = 0;
                btn.innerHTML = '<i class="fas fa-location-arrow"></i> INITIATE BROADCAST';
                btn.style.background = "rgba(0,0,0,0.5)"; btn.style.color = "var(--neon-cyan)"; btn.style.borderColor = "var(--neon-cyan)";
                metrics.style.display = 'none'; locDisp.style.display = 'none';
                db.ref(`trackings/${currentAdminTargetPhone}/location/status`).set('offline').catch(err => console.warn(err)); 
                showToast("Transmission Terminated", "error");
            } else {
                if(!navigator.geolocation) { showToast("Hardware GPS not supported.", "error"); return; }
                btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> ACQUIRING SATELLITES...';
                
                geoWatchId = navigator.geolocation.watchPosition(
                    async (pos) => {
                        btn.innerHTML = '<i class="fas fa-times-circle"></i> STOP BROADCASTING';
                        btn.style.background = "rgba(255,51,51,0.1)"; btn.style.color = "var(--danger)"; btn.style.borderColor = "var(--danger)";
                        metrics.style.display = 'grid'; locDisp.style.display = 'flex';
                        
                        pingCount++;
                        const speedKmh = pos.coords.speed ? (pos.coords.speed * 3.6).toFixed(1) : 0;
                        document.getElementById('a-speed').innerText = speedKmh + " km/h";
                        document.getElementById('a-acc').innerText = "±" + Math.round(pos.coords.accuracy) + "m";
                        document.getElementById('a-ping').innerText = pingCount;

                        globalAdminLat = pos.coords.latitude; globalAdminLng = pos.coords.longitude;
                        updateLiveRouteMap('admin');

                        let locName = "Resolving...";
                        if(pingCount % 10 === 1) { 
                            locName = await fetchLocationName(pos.coords.latitude, pos.coords.longitude);
                            document.getElementById('a-loc-name').innerText = escapeHTML(locName);
                        } else { locName = document.getElementById('a-loc-name').innerText; }

                        db.ref(`trackings/${currentAdminTargetPhone}/location`).set({
                            status: 'online', lat: pos.coords.latitude, lng: pos.coords.longitude,
                            speed: speedKmh, heading: pos.coords.heading ? Math.round(pos.coords.heading) + "°" : "N/A",
                            accuracy: Math.round(pos.coords.accuracy), time: new Date().toLocaleTimeString(), locationName: locName
                        });
                        
                        if(pingCount === 1) showToast("Satellite Uplink Established");
                    },
                    (err) => {
                        showToast("GPS Error. Ensure Location Services are ON.", "error");
                        if(geoWatchId !== null) navigator.geolocation.clearWatch(geoWatchId);
                        geoWatchId = null; btn.innerHTML = '<i class="fas fa-location-arrow"></i> INITIATE BROADCAST';
                    }, 
                    { enableHighAccuracy: true, maximumAge: 0, timeout: 15000 }
                );
            }
        }

        // --- ADMIN: TARGETED OFFICIAL UPDATE ---
        function setQuickUpdate(msg, e) { if(e) e.stopPropagation(); document.getElementById('admin-update-text').value = msg; }
        function adminPushUpdate(e) {
            if(e) e.stopPropagation(); if(!currentAdminTargetPhone) return;
            const text = document.getElementById('admin-update-text').value.trim();
            const btn = document.getElementById('btn-push-update');
            if(!text) { showToast("Enter an official update to transmit.", "error"); return; }

            btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> TRANSMITTING...'; btn.disabled = true;

            db.ref(`trackings/${currentAdminTargetPhone}/updates`).push({
                text: text, time: new Date().toLocaleTimeString('en-US', {hour: '2-digit', minute:'2-digit'}), timestamp: Date.now()
            }).then(() => {
                document.getElementById('admin-update-text').value = '';
                btn.innerHTML = '<i class="fas fa-paper-plane"></i> TRANSMIT'; btn.disabled = false;
                showToast("Official Update Transmitted");
            }).catch(err => { showToast("Permission Denied", "error"); btn.disabled = false; });
        }
        function adminDeleteUpdate(key, e) {
            if(e) e.stopPropagation(); if(!currentAdminTargetPhone) return;
            db.ref(`trackings/${currentAdminTargetPhone}/updates/${key}`).remove()
            .then(() => showToast("Update Deleted")).catch(e => showToast("Failed to delete", "error"));
        }

        // --- CLIENT: TARGETED WATCH LISTENERS & PUSH NOTIFICATIONS ---
        function startClientWatchListeners() {
            if(!currentClientPhone) return;

            db.ref(`trackings/${currentClientPhone}/admin_presence`).on('value', snap => {
                const data = snap.val(); const statusEl = document.getElementById('client-chat-status');
                if(!data) { statusEl.innerText = "Offline"; statusEl.style.color = "#888"; return; }
                if(data.status === 'online') {
                    statusEl.innerText = "Online"; statusEl.style.color = "var(--neon-green)";
                } else {
                    const date = new Date(data.lastSeen);
                    statusEl.innerText = "last seen " + date.toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
                    statusEl.style.color = "#888";
                }
            });

            db.ref(`trackings/${currentClientPhone}/client_location`).on('value', (snap) => {
                if(snap.exists()) {
                    globalClientEventLat = snap.val().lat; globalClientEventLng = snap.val().lng;
                    updateLiveRouteMap('client');
                }
            });

            db.ref(`trackings/${currentClientPhone}/location`).on('value', (snapshot) => {
                const badge = document.getElementById('client-conn-status');
                if(snapshot.exists() && snapshot.val().status === 'online') {
                    const data = snapshot.val();
                    globalAdminLat = data.lat; globalAdminLng = data.lng;
                    updateLiveRouteMap('client');
                    
                    badge.innerHTML = '<i class="fas fa-circle fa-beat"></i> ONLINE'; badge.className = 'status-badge';
                    document.getElementById('client-gps-time').innerHTML = `<span style="color:var(--neon-green);">Signal Locked (${escapeHTML(data.time)})</span>`;
                    document.getElementById('c-speed').innerText = data.speed + " km/h";
                    document.getElementById('c-heading').innerText = data.heading;
                    document.getElementById('c-accuracy').innerText = "±" + data.accuracy + "m";
                    
                    let loader = document.getElementById('map-loader-text');
                    if (loader) loader.style.display = 'none';
                    
                    let extMapLink = document.getElementById('ext-map-link');
                    if(extMapLink) {
                        extMapLink.style.display = 'block';
                        extMapLink.href = `https://www.google.com/maps/search/?api=1&query=${data.lat},${data.lng}`;
                    }

                    const locDisp = document.getElementById('client-location-display');
                    if(data.locationName && data.locationName !== "Resolving...") {
                        locDisp.style.display = 'flex'; document.getElementById('c-location-name').innerText = escapeHTML(data.locationName);
                    }

                    if(Math.abs(data.lat - lastKnownLat) > 0.0001 || Math.abs(data.lng - lastKnownLng) > 0.0001) {
                        const mapUrl = `https://maps.google.com/maps?q=${data.lat},${data.lng}&z=15&output=embed`;
                        let cIframe = document.getElementById('client-map-iframe');
                        if (cIframe) cIframe.src = mapUrl;
                        lastKnownLat = data.lat; lastKnownLng = data.lng;
                    }
                } else {
                    badge.innerHTML = '<i class="fas fa-exclamation-circle"></i> SIGNAL LOST'; badge.className = 'status-badge offline';
                    document.getElementById('client-gps-time').innerText = "Target Offline";
                }
            });

            db.ref(`trackings/${currentClientPhone}/updates`).orderByChild('timestamp').limitToLast(50).on('value', (snapshot) => {
                const feedArea = document.getElementById('client-updates-area');
                if(snapshot.exists()) {
                    let html = ''; const updates = [];
                    snapshot.forEach(child => {
                        const key = child.key; const updateData = child.val(); updates.push({key, ...updateData});
                        if (!isInitialLoad && !processedUpdateKeys.has(key)) { showPushNotification("Official Logistics Update", updateData.text); }
                        processedUpdateKeys.add(key);
                    });
                    
                    isInitialLoad = false; 
                    updates.reverse().forEach((update) => {
                        html += `<div class="timeline-item"><div class="update-content"><div class="update-time">${escapeHTML(update.time)}</div><div class="update-text">${escapeHTML(update.text)}</div></div></div>`;
                    });
                    feedArea.innerHTML = html;
                } else {
                    feedArea.innerHTML = '<div style="text-align:center; color:#555; font-size:13px; padding:20px; border:none; margin-left:-20px;">Awaiting communications...</div>';
                    isInitialLoad = false; 
                }
            });

            db.ref(`trackings/${currentClientPhone}/client_messages`).orderByChild('timestamp').limitToLast(100).on('value', (snap) => {
                const area = document.getElementById('client-chat-area');
                if(snap.exists()) {
                    let html = ''; let updatesToRead = {}; window.chatMessages = window.chatMessages || {};
                    snap.forEach(child => {
                        const key = child.key; const m = child.val();
                        window.chatMessages[key] = m; 

                        if(m.sender === 'admin' && m.status !== 'read') {
                            updatesToRead[`${key}/status`] = 'read'; updatesToRead[`${key}/readTime`] = new Date().toLocaleTimeString('en-US', {hour: '2-digit', minute:'2-digit'});
                        }

                        if (!isChatInitialLoad && !processedChatKeys.has(key) && m.sender === 'admin') {
                            let notifBody = m.type ? `Sent an attachment 📎` : m.text; showPushNotification("New Message from HQ", notifBody);
                        }
                        processedChatKeys.add(key);

                        const isMe = m.sender === 'client'; const bubbleClass = isMe ? 'sent' : 'received';
                        
                        const safeTextUI = escapeHTML(m.text || ''); const safeTextJS = (m.text||'').replace(/'/g, "\\'").replace(/"/g, "\\\"");
                        
                        let replyHtml = '';
                        if(m.replyToText) {
                            const senderName = m.replyToSender === 'client' ? 'You' : 'HQ Dispatch';
                            replyHtml = `<div class="chat-reply-context"><div class="sender">${senderName}</div>${escapeHTML(m.replyToText)}</div>`;
                        }

                        const editedHtml = m.edited ? `<span style="font-size:9px; opacity:0.6; font-style:italic; margin-left:5px;">(edited)</span>` : '';

                        let messageContentHtml = '';
                        if (m.type === 'image' || m.type === 'camera') {
                            messageContentHtml = `<img src="${escapeHTML(m.mediaUrl)}" style="max-width: 100%; border-radius: 8px; cursor: pointer; display:block; border: 1px solid rgba(255,255,255,0.1);" onclick="openPremiumMediaViewer('${escapeHTML(m.mediaUrl)}', 'image', 'Image Media')">`;
                        } else if (m.type === 'audio') {
                            messageContentHtml = `<audio controls src="${escapeHTML(m.mediaUrl)}" style="max-width: 220px; height: 35px; outline: none; margin:5px 0;"></audio>`;
                        } else if (m.type === 'document') {
                            messageContentHtml = `<div style="display:flex; align-items:center; gap:10px; background:rgba(0,0,0,0.4); padding:12px; border-radius:8px; border: 1px solid var(--neon-cyan); color:var(--neon-cyan); cursor:pointer; margin:5px 0;" onclick="openPremiumMediaViewer('${escapeHTML(m.mediaUrl)}', 'document', '${escapeHTML(m.fileName || 'Document')}')">
                                <i class="fas fa-file-pdf" style="font-size:24px;"></i>
                                <div>
                                    <div style="font-weight:bold; font-size:13px; color:#fff;">${escapeHTML(m.fileName || 'Document')}</div>
                                    <div style="font-size:10px; color:#aaa;">Tap to view securely</div>
                                </div>
                            </div>`;
                        } else if (m.type === 'contact') {
                            messageContentHtml = `<div style="display:flex; align-items:center; gap:12px; background:rgba(0,0,0,0.3); padding:10px; border-radius:8px; margin:5px 0; min-width: 150px;"><div style="width:40px; height:40px; min-width:40px; border-radius:50%; background:#3b82f6; display:flex; justify-content:center; align-items:center; color:#fff; font-size:18px;"><i class="fas fa-user"></i></div><div><div style="font-weight:bold; font-size:14px; color:#fff;">${escapeHTML(m.contactName)}</div><a href="tel:${escapeHTML(m.contactPhone)}" style="color:var(--neon-cyan); font-size:12px; text-decoration:none;">${escapeHTML(m.contactPhone)}</a></div></div>`;
                        } else if (m.type === 'location') {
                            messageContentHtml = `
                            <a href="https://www.google.com/maps/search/?api=1&query=${m.lat},${m.lng}" target="_blank" style="display:block; text-decoration:none; color:inherit; margin:5px 0;">
                                <div style="background:#111b21; border-radius:10px; overflow:hidden; position:relative; width:100%; max-width:260px; height:130px; border:1px solid rgba(0, 229, 255, 0.2); display:flex; flex-direction:column; justify-content:center; align-items:center;">
                                    <div style="position:absolute; inset:0; background-image:linear-gradient(rgba(0, 229, 255, 0.05) 1px, transparent 1px), linear-gradient(90deg, rgba(0, 229, 255, 0.05) 1px, transparent 1px); background-size:15px 15px; opacity:0.6;"></div>
                                    <div style="position:absolute; width:60px; height:60px; border-radius:50%; background:rgba(0, 229, 255, 0.15); border:1px solid rgba(0, 229, 255, 0.4); animation: targetPulse 1.5s infinite alternate;"></div>
                                    <i class="fas fa-map-marker-alt" style="color:var(--neon-cyan); font-size:24px; position:relative; z-index:2; text-shadow:0 0 10px var(--neon-cyan); transform:translateY(-5px);"></i>
                                    <span style="position:relative; z-index:2; color:var(--neon-cyan); font-family:'Orbitron', sans-serif; font-size:11px; font-weight:bold; margin-top:8px; text-shadow:0 0 5px rgba(0,229,255,0.5);">LOCATION LOCKED</span>
                                </div>
                            </a>`;
                        } else if (m.type === 'sticker') {
                            messageContentHtml = `<img src="${escapeHTML(m.mediaUrl)}" style="width: 120px; height: 120px; background: transparent; display:block;">`;
                        } else {
                            messageContentHtml = `<div>${safeTextUI} ${editedHtml}</div>`; 
                        }

                        let statusHtml = '';
                        if(isMe) {
                            if(m.status === 'read') statusHtml = '<span class="msg-status read"><i class="fas fa-check-double"></i></span>';
                            else if(m.status === 'delivered') statusHtml = '<span class="msg-status"><i class="fas fa-check-double"></i></span>';
                            else statusHtml = '<span class="msg-status"><i class="fas fa-check"></i></span>';
                        }

                        html += `
                            <div class="bubble-wrapper">
                                <div class="reply-swipe-icon"><i class="fas fa-reply"></i></div>
                                <div class="chat-bubble ${bubbleClass}" id="cmsg-${key}"
                                    data-key="${key}" data-text="${safeTextJS}" data-sender="${m.sender}" 
                                    data-time="${escapeHTML(m.time)}" data-readtime="${escapeHTML(m.readTime||'')} " 
                                    data-status="${m.status}" data-isme="${isMe}" data-msgtype="${m.type||'text'}"
                                    onmousedown="handleMouseDown(event, this, 'client')" onmouseup="handleMouseUp(event)" onmouseleave="handleMouseLeave(event)"
                                    ontouchstart="handleTouchStart(event, this, 'client')" ontouchmove="handleTouchMove(event)" ontouchend="handleTouchEnd(event, this, 'client')">
                                    ${replyHtml}
                                    ${messageContentHtml}
                                    <div class="chat-time">${escapeHTML(m.time)} ${statusHtml}</div>
                                </div>
                            </div>
                        `;
                    });
                    
                    if(Object.keys(updatesToRead).length > 0) { db.ref(`trackings/${currentClientPhone}/client_messages`).update(updatesToRead); }
                    isChatInitialLoad = false; area.innerHTML = html; 
                    if(document.getElementById('client-chat-container').classList.contains('chat-fullscreen')){
                        area.scrollTop = area.scrollHeight; 
                    }
                } else {
                    area.innerHTML = '<div style="font-size:12px; color:rgba(255,255,255,0.5); text-align:center; margin: auto; background:rgba(0,0,0,0.4); padding:6px 12px; border-radius:12px;">End-to-end encrypted chat started</div>';
                    isChatInitialLoad = false;
                }
            });
        }

        // --- GLOBAL LOGOUT ---
        function systemLogout(e) {
            if(e) e.stopPropagation();
            if(geoWatchId !== null) toggleLocationBroadcast();
            
            // Clear auto-login credentials
            localStorage.removeItem('mnd_auth_phone');
            localStorage.removeItem('mnd_auth_pin');
            
            db.ref('trackings').off(); 
            if(currentClientPhone) {
                db.ref(`trackings/${currentClientPhone}/location`).off();
                db.ref(`trackings/${currentClientPhone}/client_location`).off();
                db.ref(`trackings/${currentClientPhone}/updates`).off();
                db.ref(`trackings/${currentClientPhone}/client_messages`).off();
                db.ref(`trackings/${currentClientPhone}/admin_presence`).off();
                db.ref(`trackings/${currentClientPhone}/client_presence`).set({status: 'offline', lastSeen: Date.now()});
                if(RTC.callRef) RTC.callRef.off();
            }
            if(currentAdminTargetPhone) {
                db.ref(`trackings/${currentAdminTargetPhone}/client_location`).off();
                db.ref(`trackings/${currentAdminTargetPhone}/updates`).off();
                db.ref(`trackings/${currentAdminTargetPhone}/client_messages`).off();
                db.ref(`trackings/${currentAdminTargetPhone}/client_presence`).off();
                db.ref(`trackings/${currentAdminTargetPhone}/admin_presence`).set({status: 'offline', lastSeen: Date.now()});
                if(RTC.callRef) RTC.callRef.off();
            }
            
            endCallLocal();
            currentAdminTargetPhone = ""; currentClientPhone = "";
            cancelReply('admin'); cancelReply('client');
            
            let cMapIframe = document.getElementById('client-map-iframe');
            if (cMapIframe) cMapIframe.src = "";
            
            let mapLoader = document.getElementById('map-loader-text');
            if (mapLoader) mapLoader.style.display = 'block';
            
            let locDisplay = document.getElementById('client-location-display');
            if (locDisplay) locDisplay.style.display = 'none';
            
            let mapLink = document.getElementById('ext-map-link');
            if (mapLink) mapLink.style.display = 'none';
            
            let gpsTime = document.getElementById('client-gps-time');
            if (gpsTime) gpsTime.innerText = "Awaiting Signal...";
            
            showToast("Session Terminated", "error");
            switchView('view-gatekeeper');
        }
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mikro RX1000 Ultimate Simulator & Manual</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@400;700&family=Orbitron:wght@700&family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    
    <style>
        /* --- GAYA ASAS --- */
        body { background-color: #f0f2f5; font-family: 'Inter', sans-serif; color: #333; scroll-behavior: smooth; }
        
        /* --- 1. SIMULATOR PANEL --- */
        .device-casing {
            width: 560px;
            height: 520px;
            background-color: #1a1a1a;
            border-radius: 6px;
            padding: 18px;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.7), inset 0 0 20px rgba(0,0,0,0.5);
            position: relative;
            margin: 0 auto;
        }

        .faceplate {
            width: 100%;
            height: 100%;
            display: flex;
            border: 1px solid rgba(255,255,255,0.2);
            position: relative;
            overflow: hidden;
            background: #000;
        }

        /* Panel Kiri (Menu Senarai) */
        .panel-left {
            width: 42%;
            background-color: #000;
            color: #e5e5e5;
            padding: 15px 12px;
            font-family: 'Roboto Condensed', sans-serif;
            border-right: 1px solid #444;
            display: flex;
            flex-direction: column;
            z-index: 10;
        }

        .logo {
            font-family: 'Inter', sans-serif;
            font-weight: 900;
            font-size: 28px;
            color: #fff;
            letter-spacing: -1px;
            line-height: 1;
        }
        .logo-line { height: 3px; background: #ef4444; width: 55px; margin-bottom: 10px; margin-top: 2px; }

        .list-header {
            color: #9ca3af;
            font-size: 10px;
            font-weight: bold;
            text-transform: uppercase;
            border-bottom: 1px solid #333;
            padding-bottom: 2px;
            margin-bottom: 4px;
            margin-top: 10px;
        }
        .list-header:first-child { margin-top: 0; }

        .menu-item { white-space: nowrap; overflow: hidden; text-overflow: ellipsis; color: #d1d5db; display: flex; font-size: 9px; line-height: 1.4; }
        .id-red { color: #ef4444; font-weight: bold; margin-right: 4px; min-width: 12px; }
        
        .sub-item { padding-left: 14px; font-size: 8.5px; color: #9ca3af; display: flex; line-height: 1.3; }
        .sub-id { color: #ef4444; margin-right: 4px; font-weight: bold; }

        /* Panel Kanan (Kawalan) */
        .panel-right {
            width: 58%;
            background: linear-gradient(135deg, #005580 0%, #00334d 100%);
            padding: 15px 20px;
            position: relative;
            display: flex;
            flex-direction: column;
        }

        .model-title { text-align: right; color: #fff; line-height: 1; margin-bottom: 2px; }
        .model-rx { font-size: 24px; font-weight: bold; font-family: 'Roboto Condensed', sans-serif; letter-spacing: 1px; }
        .model-sub { font-size: 8px; color: #bae6fd; text-transform: uppercase; letter-spacing: 0.5px; }

        /* LED & Labels */
        .led-group {
            position: absolute;
            top: 55px;
            right: 20px;
            background: rgba(0,0,0,0.4);
            padding: 5px 10px;
            border-top: 2px solid #fff;
            display: flex;
            gap: 12px;
            border-radius: 2px;
        }
        .led-col { text-align: center; }
        .led-label { font-size: 8px; color: #fff; font-weight: bold; margin-bottom: 3px; }
        .led-bulb { width: 10px; height: 4px; background: #333; border-radius: 1px; transition: all 0.1s; margin: 0 auto; }
        .led-bulb.green.on { background: #22c55e; box-shadow: 0 0 6px #22c55e; }
        .led-bulb.red.on { background: #ef4444; box-shadow: 0 0 6px #ef4444; }

        .output-labels { position: absolute; top: 95px; right: 20px; display: flex; flex-direction: column; gap: 8px; }
        .p-box { background: #fff; color: #000; font-size: 10px; font-weight: bold; padding: 3px 8px; width: 90px; display: flex; align-items: center; border-radius: 1px; box-shadow: 1px 1px 2px rgba(0,0,0,0.3); }
        .p-dot { width: 6px; height: 6px; background: #000; margin-right: 8px; }

        /* Skrin Digital */
        .display-screen {
            background: rgba(0,0,0,0.6);
            border: 1px solid #666;
            width: 150px;
            height: 55px;
            margin-top: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 4px;
            box-shadow: inset 0 0 15px rgba(0,0,0,0.8);
        }
        .seg-text {
            font-family: 'Orbitron', monospace;
            font-size: 40px;
            color: #ff3333;
            text-shadow: 0 0 8px rgba(255, 51, 51, 0.6);
            letter-spacing: 2px;
        }

        /* NFC Icon */
        .nfc-zone {
            position: absolute;
            top: 140px;
            right: 25px;
            border: 1px solid rgba(255,255,255,0.3);
            border-radius: 6px;
            width: 35px;
            height: 45px;
            display: flex;
            align-items: flex-end;
            justify-content: center;
            padding-bottom: 4px;
            color: #fff;
            font-size: 8px;
            font-weight: bold;
        }
        .nfc-chip {
            position: absolute;
            top: 8px;
            width: 18px;
            height: 18px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 3px;
            background: rgba(255,255,255,0.05);
        }

        /* Butang */
        .arrow-btn { width: 0; height: 0; cursor: pointer; filter: drop-shadow(0 3px 3px rgba(0,0,0,0.5)); transition: transform 0.1s; }
        .arrow-btn:active { transform: scale(0.9); }
        .arrow-up { border-left: 14px solid transparent; border-right: 14px solid transparent; border-bottom: 22px solid #fff; }
        .arrow-down { border-left: 14px solid transparent; border-right: 14px solid transparent; border-top: 22px solid #fff; }
        
        .round-btn {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: radial-gradient(circle at 30% 30%, #f3f4f6, #9ca3af);
            border: 2px solid #d1d5db;
            box-shadow: 0 5px 10px rgba(0,0,0,0.5);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            font-weight: bold;
            text-align: center;
            line-height: 1.1;
            position: relative;
            transition: transform 0.1s;
            color: #1f2937;
        }
        .round-btn:active { transform: translateY(2px); box-shadow: 0 2px 4px rgba(0,0,0,0.5); }
        .btn-text-red { color: #dc2626; }

        /* --- 2. MANUAL & PHONE --- */
        .manual-container { background-color: #fff; border-radius: 12px; box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1); overflow: hidden; margin-top: 4rem; border: 1px solid #e5e7eb; }
        .manual-header-bg { background: linear-gradient(to right, #1e293b, #334155); color: white; padding: 24px; border-bottom: 4px solid #ef4444; }
        .section-title { color: #1e40af; font-weight: 800; font-size: 1.25rem; margin-bottom: 1.5rem; display: flex; align-items: center; border-bottom: 2px solid #f1f5f9; padding-bottom: 0.5rem; }
        .section-icon { background: #dbeafe; color: #1e40af; width: 32px; height: 32px; display: flex; align-items: center; justify-content: center; border-radius: 8px; margin-right: 12px; font-size: 0.9rem; font-weight: bold; }

        /* Phone Simulation */
        .phone-mockup { width: 280px; height: 560px; background: #111; border-radius: 36px; border: 8px solid #2d2d2d; position: relative; overflow: hidden; box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5); margin: 0 auto; }
        .phone-notch { position: absolute; top: 0; left: 50%; transform: translateX(-50%); width: 120px; height: 24px; background: #2d2d2d; border-radius: 0 0 16px 16px; z-index: 20; }
        .phone-screen { width: 100%; height: 100%; background: #f8fafc; display: flex; flex-direction: column; font-family: sans-serif; }
        .app-header { background: #005eb8; color: white; padding: 40px 16px 16px; font-weight: 600; display: flex; justify-content: space-between; align-items: center; }
        .app-content { padding: 20px; flex-grow: 1; display: flex; flex-direction: column; justify-content: center; }
        .app-nfc-btn { background: white; border: 2px dashed #005eb8; border-radius: 16px; padding: 30px 20px; text-align: center; cursor: pointer; color: #005eb8; font-weight: 700; margin-bottom: 20px; transition: all 0.2s; display: flex; flex-direction: column; align-items: center; }
        .app-nfc-btn:active { background: #eff6ff; transform: scale(0.98); border-style: solid; }
        
        /* Custom Menu List for Manual */
        .mt-item { margin-bottom: 8px; font-family: 'Roboto Condensed', sans-serif; color: #475569; font-size: 0.9rem; }
        .mt-code { background: #fee2e2; color: #b91c1c; padding: 1px 6px; border-radius: 4px; font-weight: bold; margin-right: 8px; font-family: monospace; }
        .mt-sub { margin-left: 36px; border-left: 2px solid #e2e8f0; padding-left: 10px; font-size: 0.85rem; color: #64748b; margin-top: 2px; }
        .mt-sub div { margin-bottom: 2px; }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-5xl mx-auto">
        
        <!-- BAHAGIAN 1: SIMULATOR -->
        <div class="mb-16">
            <div class="text-center mb-8">
                <h1 class="text-4xl font-bold text-gray-800 mb-2">Mikro RX1000</h1>
                <p class="text-gray-500 font-medium">Simulator Panel & Manual Operasi Digital</p>
            </div>

            <div class="device-casing">
                <div class="faceplate">
                    
                    <!-- PANEL KIRI: MENU -->
                    <div class="panel-left">
                        <div class="logo">Mikro<span style="font-size: 12px; vertical-align: top; color: #999; margin-left: 2px;">®</span></div>
                        <div class="logo-line"></div>

                        <div class="list-header">MEASUREMENT</div>
                        <div class="menu-item"><span class="id-red">0</span>-Io <span class="id-red ml-2">1</span>-IL1 <span class="id-red ml-2">2</span>-IL2 <span class="id-red ml-2">3</span>-IL3</div>
                        <div class="menu-item"><span class="id-red">t</span>-Thermal %</div>

                        <div class="list-header mt-2">MENU</div>
                        <!-- Struktur Menu Tepat seperti diminta -->
                        <div class="menu-item"><span class="id-red">A.</span>Overcurrent Setting</div>
                        <div class="menu-item"><span class="id-red">b.</span>Earth-Fault Setting</div>
                        
                        <div class="sub-item"><span class="sub-id">1</span>-3I>/Io> Protection</div>
                        <div class="sub-item"><span class="sub-id">2</span>-3I>/Io> Delay Type</div>
                        <div class="sub-item"><span class="sub-id">3</span>-tI>(Seconds)/ktI></div>
                        <div class="sub-item"><span class="sub-id">4</span>-3I>>/Io>> Protection</div>
                        <div class="sub-item"><span class="sub-id">5</span>-tI>>(Seconds)</div>
                        <div class="sub-item"><span class="sub-id">6</span>-3I>>> Protection</div>
                        <div class="sub-item"><span class="sub-id">7</span>-tI>>> (Seconds)</div>
                        
                        <div class="menu-item mt-1"><span class="id-red">C.</span>Thermal Overload Setting</div>
                        <div class="menu-item"><span class="id-red">d.</span>Cold Load Pickup Setting</div>
                        <div class="menu-item"><span class="id-red">E.</span>Functions Setting</div>
                        <div class="menu-item"><span class="id-red">F.</span>Date and Time Setting</div>
                    </div>

                    <!-- PANEL KANAN: KAWALAN -->
                    <div class="panel-right">
                        <div class="model-title">
                            <div class="model-rx">RX 1000</div>
                            <div class="model-sub">COMBINED OVERCURRENT & EARTH FAULT RELAY</div>
                        </div>

                        <div class="led-group">
                            <div class="led-col"><div class="led-label">AUX</div><div class="led-bulb green on" id="led-aux"></div></div>
                            <div class="led-col"><div class="led-label">I></div><div class="led-bulb red" id="led-i1"></div></div>
                            <div class="led-col"><div class="led-label">I>></div><div class="led-bulb red" id="led-i2"></div></div>
                        </div>

                        <div class="output-labels">
                            <div class="p-box"><div class="p-dot"></div> P1</div>
                            <div class="p-box"><div class="p-dot"></div> P2</div>
                        </div>

                        <div class="display-screen">
                            <span class="seg-text" id="display-val">00.98</span>
                        </div>

                        <div class="nfc-zone">
                            <div class="nfc-chip"></div>
                            NFC
                        </div>

                        <!-- Butang Kawalan -->
                        <div class="absolute top-[230px] left-[70px] flex gap-20">
                            <div class="arrow-up arrow-btn" onclick="changeVal(1)"></div>
                            <div class="arrow-down arrow-btn" onclick="changeVal(-1)"></div>
                        </div>

                        <div class="absolute bottom-[30px] left-[50px] flex gap-12 items-end">
                            <div class="text-center">
                                <div class="round-btn" onmousedown="startTest()" onmouseup="endTest()">
                                    <span>TEST</span>
                                </div>
                                <div class="text-[8px] text-white mt-1 font-medium">Hold 3s<br>to test</div>
                            </div>
                            <div class="text-center">
                                <div class="round-btn" onmousedown="holdMode()" onmouseup="releaseMode()">
                                    <span class="btn-text-red">RESET/<br>MODE</span>
                                </div>
                                <div class="text-[8px] text-white mt-1 font-medium">Hold 1s<br>Menu</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="text-center mt-6 text-sm text-gray-500">
                👆 <strong>Simulator Interaktif:</strong> Tekan butang pada panel untuk simulasi.
            </div>
        </div>

        <!-- BAHAGIAN 2: MANUAL LENGKAP -->
        <div class="manual-container">
            <div class="manual-header-bg">
                <h2 class="text-2xl font-bold">Dokumentasi Manual</h2>
                <p class="opacity-80 text-sm tracking-wide">PANDUAN OPERASI & KONFIGURASI</p>
            </div>

            <div class="p-6 md:p-10">
                
                <!-- Pengenalan -->
                <div class="mb-10 bg-slate-50 p-6 rounded-xl border border-slate-200 text-sm text-slate-700 leading-relaxed">
                    <h3 class="font-bold text-slate-900 mb-2 text-lg">Mengenai Peranti</h3>
                    <p><strong>Mikro RX 1000</strong> adalah gabungan alat proteksi yang direka untuk melindungi daripada kebocoran arus (earth fault) dan arus berlebihan (overcurrent). Ia membantu mencegah risiko kebakaran dan kerosakan peralatan dan sesuai untuk panel elektrik di sektor industri dan komersial. Peranti ini mempunyai paparan tujuh segmen untuk memudahkan anda melihat hasil pengukuran dan menu pengaturan.</p>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-2 gap-12">
                    
                    <!-- KOLUM KIRI: PANDUAN TEKS -->
                    <div class="space-y-12">
                        
                        <!-- I. Butang Fizikal -->
                        <div>
                            <div class="section-title"><span class="section-icon">I</span>Cara Tetapan (Kaedah Butang Fizikal)</div>
                            <ul class="space-y-4 text-sm text-gray-700">
                                <li class="bg-white p-4 border rounded-lg shadow-sm flex gap-4">
                                    <div class="font-bold bg-blue-100 text-blue-700 w-8 h-8 rounded-full flex items-center justify-center flex-shrink-0">1</div>
                                    <div>
                                        <strong class="block text-gray-900 mb-1">Masuk ke Menu Pengaturan</strong>
                                        Tekan dan tahan butang <strong>"Reset/Mode"</strong> selama 1 saat untuk memasuki menu pengaturan dari menu Pengukuran.
                                    </div>
                                </li>
                                <li class="bg-white p-4 border rounded-lg shadow-sm flex gap-4">
                                    <div class="font-bold bg-blue-100 text-blue-700 w-8 h-8 rounded-full flex items-center justify-center flex-shrink-0">2</div>
                                    <div>
                                        <strong class="block text-gray-900 mb-1">Menatal Menu</strong>
                                        Tekan butang <strong>"Mode"</strong> untuk menatal (scroll) paparan antara kategori tetapan (seperti Overcurrent Setting, Earth-Fault Setting, dsb.).
                                    </div>
                                </li>
                                <li class="bg-white p-4 border rounded-lg shadow-sm flex gap-4">
                                    <div class="font-bold bg-blue-100 text-blue-700 w-8 h-8 rounded-full flex items-center justify-center flex-shrink-0">3</div>
                                    <div>
                                        <strong class="block text-gray-900 mb-1">Menyesuaikan Nilai</strong>
                                        Gunakan butang panah atas <strong>("UP")</strong> untuk menaikkan nilai dan butang panah bawah <strong>("DOWN")</strong> untuk menurunkan nilai.
                                    </div>
                                </li>
                                <li class="bg-green-50 p-4 border border-green-100 rounded-lg shadow-sm flex gap-4">
                                    <div class="font-bold bg-green-100 text-green-700 w-8 h-8 rounded-full flex items-center justify-center flex-shrink-0">4</div>
                                    <div>
                                        <strong class="block text-green-800 mb-1">Menyimpan Tetapan</strong>
                                        Tekan butang <strong>"UP"</strong> dan <strong>"DOWN"</strong> secara serentak (simultaneously) untuk menetapkan atau menyimpan nilai tetapan baru.
                                    </div>
                                </li>
                            </ul>
                            
                            <div class="mt-4 p-4 bg-gray-100 rounded-lg text-xs text-gray-600">
                                <strong>Fungsi Butang Tambahan:</strong>
                                <ul class="list-disc ml-4 mt-1 space-y-1">
                                    <li><strong>Ujian Trip (Trip Test):</strong> Tekan dan tahan butang "TEST" selama 3.5 saat.</li>
                                    <li><strong>Reset Trip:</strong> Tekan butang "RESET" untuk mengembalikan keadaan geganti (relay) selepas trip.</li>
                                    <li><strong>Keluar Mode:</strong> Butang "Reset" atau "Mod" juga berfungsi untuk keluar dari mode tes.</li>
                                </ul>
                            </div>
                        </div>

                        <!-- II. Menu Structure (Detailed) -->
                        <div>
                            <div class="section-title"><span class="section-icon">II</span>Kategori Tatapan Utama</div>
                            <div class="bg-slate-50 p-5 rounded-lg border border-slate-200">
                                
                                <div class="mt-item"><span class="mt-code">A</span> Overcurrent Setting</div>
                                
                                <div class="mt-item">
                                    <span class="mt-code">b</span> Earth-Fault Setting
                                    <div class="mt-sub">
                                        <div><span class="font-bold text-red-600">1</span> - 3I>/Io> Protection</div>
                                        <div><span class="font-bold text-red-600">2</span> - 3I>/Io> Delay Type</div>
                                        <div><span class="font-bold text-red-600">3</span> - tI>(Seconds)/ktI></div>
                                        <div><span class="font-bold text-red-600">4</span> - 3I>>/Io>> Protection</div>
                                        <div><span class="font-bold text-red-600">5</span> - tI>>(Seconds)</div>
                                        <div><span class="font-bold text-red-600">6</span> - 3I>>> Protection</div>
                                        <div><span class="font-bold text-red-600">7</span> - tI>>> (Seconds)</div>
                                    </div>
                                </div>

                                <div class="mt-item"><span class="mt-code">C</span> Thermal Overload Setting</div>
                                <div class="mt-item"><span class="mt-code">d</span> Cold Load Pickup Setting</div>
                                <div class="mt-item"><span class="mt-code">E</span> Functions Setting</div>
                                <div class="mt-item"><span class="mt-code">F</span> Date and Time Setting</div>
                            </div>
                        </div>

                    </div>

                    <!-- KOLUM KANAN: NFC & PHONE -->
                    <div>
                        <div class="section-title"><span class="section-icon">III</span>Cara Tetapan (Kaedah NFC)</div>
                        
                        <div class="bg-white p-5 border rounded-xl mb-8 space-y-4 shadow-sm">
                            <div class="flex gap-3 items-start">
                                <span class="flex items-center justify-center w-6 h-6 rounded-full bg-blue-100 text-blue-700 text-xs font-bold flex-shrink-0">1</span>
                                <p class="text-sm text-gray-600">Muat turun aplikasi <strong>Micro NFC</strong> (Android/iOS).</p>
                            </div>
                            <div class="flex gap-3 items-start">
                                <span class="flex items-center justify-center w-6 h-6 rounded-full bg-blue-100 text-blue-700 text-xs font-bold flex-shrink-0">2</span>
                                <p class="text-sm text-gray-600">Pada relay, pastikan <strong>"NFC remote set"</strong> diaktifkan (Yes).</p>
                            </div>
                            <div class="flex gap-3 items-start">
                                <span class="flex items-center justify-center w-6 h-6 rounded-full bg-blue-100 text-blue-700 text-xs font-bold flex-shrink-0">3</span>
                                <p class="text-sm text-gray-600">Buka aplikasi dan tekan <strong>"Click to active NFC"</strong>.</p>
                            </div>
                            <div class="flex gap-3 items-start">
                                <span class="flex items-center justify-center w-6 h-6 rounded-full bg-blue-100 text-blue-700 text-xs font-bold flex-shrink-0">4</span>
                                <p class="text-sm text-gray-600">Dekatkan telefon ke logo <strong>NFC</strong> pada panel RX1000. Pastikan sama seperti aplikasi NFC makro RX1000 di handphone dan boleh masukkan data.</p>
                            </div>
                        </div>

                        <div class="bg-blue-50 p-4 rounded-lg text-xs text-blue-800 mb-8 border border-blue-100">
                            <strong>Melalui aplikasi Micro NFC, anda boleh:</strong>
                            <ol class="list-decimal ml-4 mt-2 space-y-1 text-slate-700">
                                <li>Mengubah nilai pengaturan dari jarak jauh.</li>
                                <li>Melihat nilai pengukuran, status penggera, dan output R1 R2.</li>
                                <li>Mengambil data rekod (seperti 30 nilai pengukuran, 30 status penggera, dan 120 kegiatan/perubahan nilai) dan menyimpannya ke dalam telefon pintar.</li>
                            </ol>
                        </div>

                        <!-- Phone Simulation -->
                        <div class="flex flex-col items-center">
                            <h4 class="text-xs font-bold text-gray-400 uppercase mb-4 tracking-widest">Simulasi Aplikasi Telefon</h4>
                            <div class="phone-mockup">
                                <div class="phone-notch"></div>
                                <div class="phone-screen">
                                    <div class="app-header">
                                        <span>Micro NFC</span>
                                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path></svg>
                                    </div>
                                    
                                    <div class="app-content">
                                        <div class="flex-grow flex flex-col justify-center items-center mb-6">
                                            <div class="w-20 h-20 bg-blue-50 rounded-full flex items-center justify-center mb-4">
                                                <svg class="w-10 h-10 text-blue-600" fill="currentColor" viewBox="0 0 24 24"><path d="M20 2H4c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm0 18H4V4h16v16zM18 6h-5c-1.1 0-2 .9-2 2v2.28c-.6.35-1 .98-1 1.72 0 1.1.9 2 2 2s2-.9 2-2c0-.74-.4-1.38-1-1.72V8h3v8H8V8h2V6H6v12h12V6z"/></svg>
                                            </div>
                                            <h3 class="text-xl font-bold text-gray-800">RX 1000</h3>
                                            <p class="text-xs text-gray-500 mt-1">Status: Ready to Connect</p>
                                        </div>

                                        <!-- Butang NFC Aktif -->
                                        <div class="app-nfc-btn" onclick="alert('Simulasi: Sedang mengimbas... Sila dekatkan pada panel.')">
                                            <svg class="w-10 h-10 text-blue-600 mb-2" viewBox="0 0 24 24" fill="currentColor">
                                                <path d="M20 2H4c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm0 18H4V4h16v16zM18 6h-5c-1.1 0-2 .9-2 2v2.28c-.6.35-1 .98-1 1.72 0 1.1.9 2 2 2s2-.9 2-2c0-.74-.4-1.38-1-1.72V8h3v8H8V8h2V6H6v12h12V6z"/>
                                            </svg>
                                            Click to active NFC
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // --- LOGIK SIMULATOR (Javascript) ---
        const display = document.getElementById('display-val');
        const ledAux = document.getElementById('led-aux');
        const ledI1 = document.getElementById('led-i1');
        const ledI2 = document.getElementById('led-i2');

        let currentVal = 0.98;
        let mode = 'MEASURE'; // MEASURE or MENU
        let menuIndex = 0;
        
        // Senarai Menu Mengikut Panel Kiri
        const menuItems = [
            { code: 'A', label: 'Overcurrent' },
            { code: 'b', label: 'EarthFault' },
            { code: '1', label: '3I>/Io> Prot' },
            { code: '2', label: '3I>/Io> Dly' },
            { code: '3', label: 'tI>/ktI>' },
            { code: '4', label: '3I>>/Io>> Prot' },
            { code: '5', label: 'tI>>' },
            { code: '6', label: '3I>>> Prot' },
            { code: '7', label: 'tI>>>' },
            { code: 'C', label: 'Thermal' },
            { code: 'd', label: 'ColdLoad' },
            { code: 'E', label: 'Functions' },
            { code: 'F', label: 'DateTime' }
        ];

        let modeTimer;
        let testTimer;

        function updateScreen() {
            if (mode === 'MEASURE') {
                let s = currentVal.toFixed(2);
                if (s.length < 5) s = "0" + s;
                display.innerText = s;
                display.style.color = "#ff3333"; 
            } else {
                display.innerText = menuItems[menuIndex].code + ".Set";
                display.style.color = "#fca5a5"; 
            }
        }

        function changeVal(dir) {
            if (mode === 'MEASURE') {
                currentVal += (dir * 0.01);
                if (currentVal < 0) currentVal = 0;
            } else {
                menuIndex += dir;
                if (menuIndex < 0) menuIndex = menuItems.length - 1;
                if (menuIndex >= menuItems.length) menuIndex = 0;
            }
            updateScreen();
        }

        function holdMode() {
            modeTimer = setTimeout(() => {
                mode = (mode === 'MEASURE') ? 'MENU' : 'MEASURE';
                menuIndex = 0;
                updateScreen();
            }, 1000);
        }

        function releaseMode() { clearTimeout(modeTimer); }

        function startTest() {
            testTimer = setTimeout(() => {
                display.innerText = "TEST";
                blinkLEDs();
            }, 3000);
        }

        function endTest() { clearTimeout(testTimer); updateScreen(); }

        function blinkLEDs() {
            let count = 0;
            let interval = setInterval(() => {
                ledI1.classList.toggle('on');
                ledI2.classList.toggle('on');
                count++;
                if(count > 6) {
                    clearInterval(interval);
                    ledI1.classList.remove('on');
                    ledI2.classList.remove('on');
                    updateScreen();
                }
            }, 300);
        }

        updateScreen();
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chaat Puchka - Kitchen Login</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-slate-950 flex justify-center items-center min-h-screen m-0 p-4">
    <div class="bg-white p-8 rounded-3xl shadow-2xl w-full max-w-sm text-center border border-slate-800">
        <div class="mb-6 flex flex-col items-center">
            <div class="w-20 h-20 bg-black rounded-2xl flex items-center justify-center p-2 mb-3 shadow-md border border-slate-800">
                <img src="https://i.postimg.cc/SsSZBQ9q/1000037254-removebg-preview.png" alt="Logo" class="w-full h-full object-contain">
            </div>
            <h2 class="text-2xl font-black text-slate-900 tracking-tight">Kitchen KDS Login</h2>
            <p class="text-xs text-amber-800 font-medium mt-1">Sign in to preparation kitchen display system</p>
        </div>
        <input type="text" id="loginId" placeholder="Login ID" class="w-full p-3.5 mb-3 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-amber-500 box-border font-semibold">
        <input type="password" id="kitchenPassword" placeholder="Password" class="w-full p-3.5 mb-4 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-amber-500 box-border font-semibold">
        <button onclick="kitchenLogin()" class="w-full bg-emerald-500 hover:bg-emerald-600 text-white font-black p-3.5 rounded-xl shadow-md transition">Login to KDS</button>
        <div id="error-msg" class="text-red-500 text-xs mt-3 font-semibold"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getDatabase, ref, get, child } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

        const firebaseConfig = {
            databaseURL: "https://teat-2-4b868-default-rtdb.europe-west1.firebasedatabase.app/"
        };
        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);

        window.kitchenLogin = function() {
            const inputId = document.getElementById('loginId').value.trim();
            const password = document.getElementById('kitchenPassword').value.trim();
            const errorDiv = document.getElementById('error-msg');
            errorDiv.textContent = '';

            get(child(ref(db), 'franchises')).then((snapshot) => {
                if (snapshot.exists()) {
                    let found = false;
                    snapshot.forEach((childSnap) => {
                        let branch = childSnap.val();
                        if (branch.loginId === inputId && branch.password === password) {
                            found = true;
                            window.location.href = `https://sakshiflavor.github.io/KITCHEN-LOGIN/?branchId=${childSnap.key}&branchName=${encodeURIComponent(branch.name)}`;
                        }
                    });
                    if (!found) errorDiv.textContent = 'Invalid Login ID or Password.';
                }
            });
        };
    </script>
</body>
</html>

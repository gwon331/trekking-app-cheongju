# trekking-app-cheongju
청주콜핑산악회
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>청주콜핑산악회 어플</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700&display=swap');
        body {
            font-family: 'Noto Sans KR', sans-serif;
            background-color: #f3f4f6;
        }
        .seat-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 1rem;
            text-align: center;
        }
        .seat-table td, .seat-table th {
            border: 1px solid #d1d5db;
            padding: 0.5rem;
        }
        .seat-input {
            width: 100%;
            text-align: center;
            border: none;
            outline: none;
            background: none;
        }
        .editable-field {
            cursor: text;
            padding: 0.25rem;
            border-bottom: 1px solid transparent;
            transition: border-color 0.2s ease-in-out;
            min-height: 1.5rem;
        }
        .editable-field:focus {
            outline: none;
            border-bottom: 1px solid #6366f1;
        }
        .header-bg {
            background-image: url('https://images.unsplash.com/photo-1549880338-65ddcdfd017b?q=80&w=1470&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            color: yellow;
            padding: 2rem;
            border-radius: 1rem;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen p-4 flex flex-col items-center">
    <div class="w-full max-w-4xl bg-white p-6 rounded-2xl shadow-xl flex flex-col items-center space-y-8">
        <!-- 앱 제목 및 사용자 ID 표시 -->
        <header class="w-full text-center header-bg">
            <h1 class="text-4xl font-bold mb-2 drop-shadow-lg">⛰️ 청주콜핑산악회</h1>
            <p id="userIdDisplay" class="text-sm font-mono drop-shadow-lg"></p>
            <div id="loadingIndicator" class="text-blue-200 mt-4 hidden">
                <div class="animate-spin inline-block h-6 w-6 border-4 border-blue-200 border-solid rounded-full border-r-transparent"></div>
                <span class="ml-2">로딩 중...</span>
            </div>
        </header>

        <!-- 메뉴바 -->
        <nav class="w-full bg-indigo-600 rounded-lg shadow-md mb-6">
            <ul class="flex justify-around items-center text-white font-semibold text-lg">
                <li id="menu-schedule" class="p-4 border-b-2 border-transparent cursor-pointer hover:bg-indigo-700 transition-colors" onclick="showView('schedule')">산행안내</li>
                <li id="menu-board" class="p-4 border-b-2 border-transparent cursor-pointer hover:bg-indigo-700 transition-colors" onclick="showView('board')">게시판</li>
            </ul>
        </nav>

        <!-- 콘텐츠 섹션 -->
        <div id="scheduleSection" class="w-full space-y-6">
            <div class="bg-gray-50 p-6 rounded-xl shadow-inner border border-gray-200">
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">📢 다가오는 산행 일정</h2>
                <div class="space-y-4">
                    <div class="bg-white p-4 rounded-xl shadow border border-gray-200">
                        <h3 class="text-xl font-bold text-gray-800 mb-2">산행정보</h3>
                        <p class="text-gray-600 mb-1">
                            <strong class="font-bold">산행일:</strong>
                            <span id="tripDate" class="editable-field" contenteditable="true">oo월 00일(토) 00시 매장 앞 출발</span>
                        </p>
                        <p class="text-gray-600 mb-1">
                            <strong class="font-bold">산행지:</strong>
                            <span id="tripLocation" class="editable-field" contenteditable="true">장소</span>
                        </p>
                        <p class="text-gray-600 mb-1">
                            <strong class="font-bold">산행코스:</strong>
                            <span id="tripCourse" class="editable-field" contenteditable="true">코스</span>
                        </p>
                    </div>

                    <div class="bg-white p-4 rounded-xl shadow border border-gray-200">
                        <h3 class="text-xl font-bold text-gray-800 mb-2">준비물 및 제공</h3>
                        <p class="text-gray-600 mb-1">
                            <strong class="font-bold">준비물:</strong>
                            <span id="preparation" class="editable-field" contenteditable="true">간단한 개인 반찬 및 간식</span>
                        </p>
                        <p class="text-gray-600 mb-1">
                            <strong class="font-bold">제공:</strong>
                            <span id="supply" class="editable-field" contenteditable="true">음료, 떡, 밥, 하산주, 등산용품 경품</span>
                        </p>
                    </div>

                    <div class="bg-white p-4 rounded-xl shadow border border-gray-200">
                        <h3 class="text-xl font-bold text-gray-800 mb-2">회비 및 문의전화</h3>
                        <p class="text-gray-600 mb-1">
                            <strong class="font-bold">기본회비:</strong>
                            <span id="fee" class="editable-field" contenteditable="true">3만 5천원</span>
                            <span class="text-gray-600">추가비용: </span>
                            <span id="extraCost" class="editable-field" contenteditable="true">유무</span>
                        </p>
                        <p class="text-gray-600 mb-1">
                            <strong class="font-bold">입금계좌:</strong>
                            <span id="accountInfo" class="editable-field" contenteditable="true">농협 302-1248-7785-21 (양순자)</span>
                        </p>
                        <p class="text-gray-600 mb-1">
                            <strong class="font-bold">문의전화:</strong>
                            <span id="contactInfo" class="editable-field" contenteditable="true">콜핑: 043-264-0278, 총무 양순자 010-6242-5090</span>
                        </p>
                        <h3 class="text-xl font-bold text-gray-800 mb-2 mt-4">산행 중 비상연락처</h3>
                        <p class="text-gray-600 mb-1">
                            <span id="emergencyContact1" class="editable-field" contenteditable="true">010-5331-1675</span>
                        </p>
                        <p class="text-gray-600">
                            <span id="emergencyContact2" class="editable-field" contenteditable="true">010-5495-9603</span>
                        </p>
                    </div>
                </div>
                <div class="flex flex-col sm:flex-row justify-between items-center mt-4 gap-2">
                    <button onclick="saveTripInfo()" class="px-4 py-2 rounded-lg font-semibold bg-indigo-500 text-white hover:bg-indigo-600 transition-colors w-full sm:w-auto" disabled>정보 저장</button>
                    <div class="flex-grow"></div>
                    <span id="registeredCount" class="text-sm font-semibold text-blue-600">신청 인원: 0명</span>
                    <input id="nameInput" type="text" placeholder="이름을 입력해 주세요" class="w-full sm:w-auto p-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-shadow"/>
                    <button onclick="handleRegistration()" class="px-4 py-2 rounded-lg font-semibold bg-blue-500 text-white hover:bg-blue-600 transition-colors w-full sm:w-auto" disabled id="registrationButton">신청하기</button>
                </div>
            </div>

            <!-- 좌석 배치 섹션 -->
            <div id="seatPlanSection" class="bg-gray-50 p-6 rounded-xl shadow-inner border border-gray-200">
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">💺 좌석 배치 (총 44석)</h2>
                <div class="flex justify-center space-x-8">
                    <table class="seat-table" id="seatTable">
                        <tbody id="seatTableBody">
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- 대기자 명단 섹션 -->
            <div id="waitingListSection" class="bg-gray-50 p-6 rounded-xl shadow-inner border border-gray-200 mt-6">
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">🚶‍♀️ 대기자 명단</h2>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-gray-300 flex flex-col items-center justify-center text-sm text-gray-500 hover:bg-gray-200 transition-colors">
                        <span class="text-xs font-semibold text-gray-400 mb-1">대기자 1</span>
                        <div class="font-bold text-gray-800 cursor-pointer editable-name" data-waitlist-index="0" contenteditable="true">000</div>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-gray-300 flex flex-col items-center justify-center text-sm text-gray-500 hover:bg-gray-200 transition-colors">
                        <span class="text-xs font-semibold text-gray-400 mb-1">대기자 2</span>
                        <div class="font-bold text-gray-800 cursor-pointer editable-name" data-waitlist-index="1" contenteditable="true">000</div>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-gray-300 flex flex-col items-center justify-center text-sm text-gray-500 hover:bg-gray-200 transition-colors">
                        <span class="text-xs font-semibold text-gray-400 mb-1">대기자 3</span>
                        <div class="font-bold text-gray-800 cursor-pointer editable-name" data-waitlist-index="2" contenteditable="true">000</div>
                    </div>
                </div>
            </div>

            <!-- 산행지 미리보기 섹션 -->
            <div id="mountainPhotoSection" class="bg-gray-50 p-6 rounded-xl shadow-inner border border-gray-200 space-y-4">
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">🏞️ 산행지 미리보기</h2>
                <div class="bg-white p-4 rounded-xl shadow border border-gray-200">
                    <div class="flex flex-col sm:flex-row gap-2 mb-4">
                        <label for="mountainPhotoInput" class="px-4 py-2 rounded-lg font-semibold bg-green-500 text-white hover:bg-green-600 transition-colors cursor-pointer">사진 선택</label>
                        <input id="mountainPhotoInput" type="file" accept="image/*" class="hidden" onchange="addMountainPhoto(event)" />
                    </div>
                    <div id="mountainPhotoGallery" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
                        <!-- 산행지 사진들이 여기에 표시됩니다 -->
                    </div>
                </div>
            </div>

            <!-- 안전 산행 요령 섹션 -->
            <div id="hikingTipsSection" class="bg-gray-50 p-6 rounded-xl shadow-inner border border-gray-200 space-y-4">
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">📝 안전 산행 요령</h2>
                <div class="bg-white p-4 rounded-xl shadow border border-gray-200 space-y-4">
                    <textarea id="hikingTipsContent" class="w-full h-48 p-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-shadow" placeholder="안전 산행에 대한 중요한 요령을 여기에 작성해 주세요."></textarea>
                    <button onclick="saveHikingTips()" class="w-full px-4 py-2 rounded-lg font-semibold bg-indigo-500 text-white hover:bg-indigo-600 transition-colors">저장하기</button>
                </div>
            </div>

            <!-- 회원 갤러리 섹션 -->
            <div id="personalPhotoSection" class="bg-gray-50 p-6 rounded-xl shadow-inner border border-gray-200 space-y-4">
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">📸 회원 갤러리</h2>
                <div class="bg-white p-4 rounded-xl shadow border border-gray-200">
                    <div class="flex flex-col sm:flex-row gap-2 mb-4">
                        <label for="personalPhotoInput" class="px-4 py-2 rounded-lg font-semibold bg-blue-500 text-white hover:bg-blue-600 transition-colors cursor-pointer">사진 선택</label>
                        <input id="personalPhotoInput" type="file" accept="image/*" class="hidden" onchange="addPersonalPhoto(event)" />
                    </div>
                    <div id="personalPhotoGallery" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
                        <!-- 개인 사진들이 여기에 표시됩니다 -->
                    </div>
                </div>
            </div>
        </div>

        <!-- 게시판 섹션 -->
        <div id="boardSection" class="w-full space-y-6 hidden">
            <nav class="w-full rounded-lg shadow-md mb-6 bg-gray-100">
                <ul class="flex justify-center text-gray-600 font-semibold text-base">
                    <li id="board-free" class="p-3 border-b-2 border-transparent cursor-pointer hover:text-indigo-600 transition-colors" onclick="showBoardView('free')">자유게시판</li>
                    <li id="board-recommend" class="p-3 border-b-2 border-transparent cursor-pointer hover:text-indigo-600 transition-colors" onclick="showBoardView('recommend')">산행지 추천</li>
                </ul>
            </nav>
            
            <div id="freeBoardView" class="w-full space-y-6">
                <div class="bg-gray-50 p-6 rounded-xl shadow-inner border border-gray-200">
                    <h3 class="text-xl font-semibold text-gray-700 mb-4">📝 자유롭게 글 올리기</h3>
                    <div class="bg-white p-4 rounded-xl shadow border border-gray-200 space-y-4">
                        <textarea id="postContent" class="w-full h-24 p-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-shadow" placeholder="자유롭게 글을 작성해 주세요..."></textarea>
                        <button onclick="submitPost('free')" class="w-full px-4 py-2 rounded-lg font-semibold bg-indigo-500 text-white hover:bg-indigo-600 transition-colors">글 올리기</button>
                    </div>
                </div>
                <div class="bg-gray-50 p-6 rounded-xl shadow-inner border border-gray-200 mt-6">
                    <h3 class="text-xl font-semibold text-gray-700 mb-4">최신 글 목록</h3>
                    <div id="postsList" class="space-y-4">
                        <!-- 게시물이 여기에 동적으로 표시됩니다 -->
                        <div class="text-gray-500 text-center">아직 게시물이 없습니다.</div>
                    </div>
                </div>
            </div>

            <div id="recommendBoardView" class="w-full space-y-6 hidden">
                <div class="bg-gray-50 p-6 rounded-xl shadow-xl shadow-inner border border-gray-200">
                    <h3 class="text-xl font-semibold text-gray-700 mb-4">📝 산행지 추천 글쓰기</h3>
                    <div class="bg-white p-4 rounded-xl shadow border border-gray-200 space-y-4">
                        <input id="recommendationTitle" type="text" placeholder="제목을 입력해 주세요" class="w-full p-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-shadow">
                        <textarea id="recommendationContent" class="w-full h-24 p-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-shadow" placeholder="추천하는 산행지에 대한 내용을 작성해 주세요..."></textarea>
                        <button onclick="submitPost('recommend')" class="w-full px-4 py-2 rounded-lg font-semibold bg-indigo-500 text-white hover:bg-indigo-600 transition-colors">글 올리기</button>
                    </div>
                </div>
                <div class="bg-gray-50 p-6 rounded-xl shadow-inner border border-gray-200 mt-6">
                    <h3 class="text-xl font-semibold text-gray-700 mb-4">추천 글 목록</h3>
                    <div id="recommendationsList" class="space-y-4">
                        <!-- 추천 게시물이 여기에 동적으로 표시됩니다 -->
                        <div class="text-gray-500 text-center">아직 추천 글이 없습니다.</div>
                    </div>
                </div>
            </div>
        </div>
        
    </div>

    <!-- Modal Container -->
    <div id="messageModal" class="fixed inset-0 bg-gray-600 bg-opacity-75 flex items-center justify-center hidden z-50">
        <div class="bg-white p-6 rounded-lg shadow-xl text-center">
            <h3 id="modalMessage" class="text-xl font-bold text-gray-800 mb-4"></h3>
            <button onclick="document.getElementById('messageModal').classList.add('hidden')" class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition-colors">확인</button>
        </div>
    </div>

    <!-- Firebase SDKs -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, collection, setDoc, updateDoc, arrayUnion, arrayRemove, getDoc, setLogLevel, addDoc } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";
        
        // Firestore settings and variables
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        let app, db, auth, userId;
        const publicSchedulesPath = `/artifacts/${appId}/public/data/schedules`;
        const postsCollectionPath = `/artifacts/${appId}/public/data/posts`;
        const recommendationsCollectionPath = `/artifacts/${appId}/public/data/recommendations`;
        const tripId = 'kolping_trip';

        const loadingIndicator = document.getElementById('loadingIndicator');
        const registrationButton = document.getElementById('registrationButton');
        const registeredCountDisplay = document.getElementById('registeredCount');
        const nameInput = document.getElementById('nameInput');
        const mountainPhotoInput = document.getElementById('mountainPhotoInput');
        const mountainPhotoGallery = document.getElementById('mountainPhotoGallery');
        const hikingTipsContent = document.getElementById('hikingTipsContent');
        const personalPhotoInput = document.getElementById('personalPhotoInput');
        const personalPhotoGallery = document.getElementById('personalPhotoGallery');
        const scheduleSection = document.getElementById('scheduleSection');
        const boardSection = document.getElementById('boardSection');
        const postsList = document.getElementById('postsList');
        const menuSchedule = document.getElementById('menu-schedule');
        const menuBoard = document.getElementById('menu-board');
        const freeBoardView = document.getElementById('freeBoardView');
        const recommendBoardView = document.getElementById('recommendBoardView');
        const boardMenuFree = document.getElementById('board-free');
        const boardMenuRecommend = document.getElementById('board-recommend');
        const recommendationsList = document.getElementById('recommendationsList');
        const waitingListSection = document.getElementById('waitingListSection');
        const saveTripInfoButton = document.querySelector('button[onclick="saveTripInfo()"]');
        
        const seatTableBody = document.getElementById('seatTableBody');

        const totalSeats = 44;
        let isAuthReady = false;
        let seatAssignments = {};
        let userList = [];
        let waitingList = [];

        // 디버그 로깅 활성화
        setLogLevel('debug');

        async function initFirebase() {
            console.log("Firebase 초기화 시작.");
            loadingIndicator.classList.remove('hidden');
            try {
                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);
                
                let user;
                if (initialAuthToken) {
                    const userCredential = await signInWithCustomToken(auth, initialAuthToken);
                    user = userCredential.user;
                } else {
                    const userCredential = await signInAnonymously(auth);
                    user = userCredential.user;
                }
                
                await new Promise(resolve => {
                    const unsubscribe = onAuthStateChanged(auth, u => {
                        if (u) {
                            unsubscribe();
                            resolve(u);
                        }
                    });
                });
                
                if (user) {
                    console.log("인증 완료. 사용자 ID:", user.uid);
                    userId = user.uid;
                    document.getElementById('userIdDisplay').textContent = `사용자 ID: ${userId.slice(0, 8)}...`;
                    await new Promise(resolve => setTimeout(resolve, 300));
                    isAuthReady = true;
                    saveTripInfoButton.disabled = false;
                    registrationButton.disabled = false;
                    
                    const scheduleDocRef = doc(db, publicSchedulesPath, tripId);
                    const postsColRef = collection(db, postsCollectionPath);
                    const recommendationsColRef = collection(db, recommendationsCollectionPath);

                    const maxAttempts = 5;
                    for (let i = 0; i < maxAttempts; i++) {
                        try {
                            const docSnap = await getDoc(scheduleDocRef);
                            if (!docSnap.exists()) {
                                await setDoc(scheduleDocRef, { registeredUsers: [], assignments: {}, tripInfo: {}, hikingTips: '', mountainPhotos: [], personalPhotos: [], waitingList: [] });
                            }
                            break;
                        } catch (error) {
                            if (error.code === 'permission-denied' && i < maxAttempts - 1) {
                                await new Promise(resolve => setTimeout(resolve, 500));
                            } else {
                                throw error;
                            }
                        }
                    }

                    onSnapshot(scheduleDocRef, (docSnap) => {
                        const data = docSnap.data();
                        if (!docSnap.exists()) return;
                        
                        userList = data?.registeredUsers || [];
                        waitingList = data?.waitingList || [];
                        seatAssignments = data?.assignments || {};
                        
                        registeredCountDisplay.textContent = `신청 인원: ${userList.length}명`;
                        const currentUserInfo = userList.find(u => u.userId === userId);
                        const isRegistered = !!currentUserInfo;
                        
                        nameInput.value = currentUserInfo?.name || '';
                        registrationButton.textContent = isRegistered ? '신청 취소' : '신청하기';
                        registrationButton.classList.toggle('bg-red-500', isRegistered);
                        registrationButton.classList.toggle('hover:bg-red-600', isRegistered);
                        registrationButton.classList.toggle('bg-blue-500', !isRegistered);
                        registrationButton.classList.toggle('hover:bg-blue-600', !isRegistered);

                        updateEditableFields(data?.tripInfo);
                        updateSeatsAndWaitingList();
                        updatePhotosAndTips(data);
                    }, (error) => {
                        console.error("Firestore 실시간 리스너 오류:", error);
                        showMessageModal('데이터를 불러오는 데 실패했습니다. 잠시 후 다시 시도해 주세요.');
                    });

                    onSnapshot(postsColRef, (snapshot) => {
                        const posts = [];
                        snapshot.forEach(doc => {
                            posts.push({ id: doc.id, ...doc.data() });
                        });
                        posts.sort((a, b) => b.timestamp - a.timestamp);
                        renderPosts(posts, postsList);
                    }, (error) => {
                        console.error("게시판 리스너 오류:", error);
                    });

                    onSnapshot(recommendationsColRef, (snapshot) => {
                        const recommendations = [];
                        snapshot.forEach(doc => {
                            recommendations.push({ id: doc.id, ...doc.data() });
                        });
                        recommendations.sort((a, b) => b.timestamp - a.timestamp);
                        renderPosts(recommendations, recommendationsList, true);
                    }, (error) => {
                        console.error("추천 게시판 리스너 오류:", error);
                    });

                } else {
                    console.error("사용자 인증 실패.");
                    showMessageModal('사용자 인증에 실패했습니다. 페이지를 새로고침 해주세요.');
                }
            } catch (error) {
                console.error("Firebase 초기화 오류:", error);
                showMessageModal('오류가 발생했습니다. 콘솔을 확인해주세요.');
            } finally {
                loadingIndicator.classList.add('hidden');
                showView('schedule');
            }
        }
        
        function updateEditableFields(tripInfo) {
            if (!tripInfo) return;
            document.getElementById('tripDate').textContent = tripInfo.tripDate || 'oo월 00일(토) 00시 매장 앞 출발';
            document.getElementById('tripLocation').textContent = tripInfo.tripLocation || '장소';
            document.getElementById('tripCourse').textContent = tripInfo.tripCourse || '코스';
            document.getElementById('preparation').textContent = tripInfo.preparation || '간단한 개인 반찬 및 간식';
            document.getElementById('supply').textContent = tripInfo.supply || '음료, 떡, 밥, 하산주, 등산용품 경품';
            document.getElementById('fee').textContent = tripInfo.fee || '3만 5천원';
            document.getElementById('extraCost').textContent = tripInfo.extraCost || '유무';
            document.getElementById('accountInfo').textContent = tripInfo.accountInfo || '농협 302-1248-7785-21 (양순자)';
            document.getElementById('contactInfo').textContent = tripInfo.contactInfo || '콜핑: 043-264-0278, 총무 양순자 010-6242-5090';
            document.getElementById('emergencyContact1').textContent = tripInfo.emergencyContact1 || '010-5331-1675';
            document.getElementById('emergencyContact2').textContent = tripInfo.emergencyContact2 || '010-5495-9603';
        }
        
        async function saveTripInfo() {
            if (!isAuthReady) {
                showMessageModal('사용자 인증 중입니다. 잠시 후 다시 시도해 주세요.');
                return;
            }
            try {
                const scheduleDocRef = doc(db, publicSchedulesPath, tripId);
                const newTripInfo = {
                    tripDate: document.getElementById('tripDate').textContent,
                    tripLocation: document.getElementById('tripLocation').textContent,
                    tripCourse: document.getElementById('tripCourse').textContent,
                    preparation: document.getElementById('preparation').textContent,
                    supply: document.getElementById('supply').textContent,
                    fee: document.getElementById('fee').textContent,
                    extraCost: document.getElementById('extraCost').textContent,
                    accountInfo: document.getElementById('accountInfo').textContent,
                    contactInfo: document.getElementById('contactInfo').textContent,
                    emergencyContact1: document.getElementById('emergencyContact1').textContent,
                    emergencyContact2: document.getElementById('emergencyContact2').textContent,
                };
                await updateDoc(scheduleDocRef, { tripInfo: newTripInfo });
                showMessageModal('정보가 성공적으로 저장되었습니다.');
            } catch (error) {
                console.error("정보 저장 중 오류 발생:", error);
                showMessageModal('정보 저장 중 오류가 발생했습니다. 콘솔을 확인해주세요.');
            }
        }

        async function updateSeatsAndWaitingList() {
            // Rebuild the seat table from scratch to ensure correct structure
            const seatTableBody = document.getElementById('seatTableBody');
            seatTableBody.innerHTML = '';

            const totalRows = 11;
            for (let i = 0; i < totalRows; i++) {
                const row = document.createElement('tr');
                // Left side
                for (let j = 0; j < 2; j++) {
                    const seatNum = i * 4 + j + 1;
                    const assignedUserInfo = seatAssignments[seatNum];
                    const td = document.createElement('td');
                    td.innerHTML = `${seatNum}. <input type="text" class="seat-input" data-seat="${seatNum}" value="${assignedUserInfo ? assignedUserInfo.name : '000'}" />`;
                    if (assignedUserInfo) {
                        td.style.backgroundColor = '#bbf7d0';
                    } else {
                        td.style.backgroundColor = '#fff';
                    }
                    row.appendChild(td);
                }
                
                // Right side
                for (let j = 2; j < 4; j++) {
                    const seatNum = i * 4 + j + 1;
                    const assignedUserInfo = seatAssignments[seatNum];
                    const td = document.createElement('td');
                    td.innerHTML = `${seatNum}. <input type="text" class="seat-input" data-seat="${seatNum}" value="${assignedUserInfo ? assignedUserInfo.name : '000'}" />`;
                    if (assignedUserInfo) {
                        td.style.backgroundColor = '#bbf7d0';
                    } else {
                        td.style.backgroundColor = '#fff';
                    }
                    row.appendChild(td);
                }

                seatTableBody.appendChild(row);
            }

            // Attach blur listeners to newly created inputs
            const seats = document.querySelectorAll('.seat-input');
            seats.forEach(input => {
                input.addEventListener('blur', async (e) => {
                    const seatNum = e.target.dataset.seat;
                    const newName = e.target.value.trim();
                    const scheduleDocRef = doc(db, publicSchedulesPath, tripId);
                    const newAssignments = { ...seatAssignments };
                    
                    if (newName === '' || newName === '000') {
                        delete newAssignments[seatNum];
                    } else {
                        newAssignments[seatNum] = { userId: `manual-${Date.now()}`, name: newName };
                    }
                    
                    try {
                        await updateDoc(scheduleDocRef, { assignments: newAssignments });
                    } catch (error) {
                        console.error("좌석 업데이트 오류:", error);
                        showMessageModal('좌석 업데이트 중 오류가 발생했습니다.');
                    }
                });
            });

            // Update waiting list
            const waitingListElements = document.querySelectorAll('.editable-name');
            waitingListElements.forEach((element, index) => {
                const name = waitingList[index] || '000';
                element.textContent = name;
                if (name !== '000') {
                    element.classList.add('bg-gray-300');
                } else {
                    element.classList.remove('bg-gray-300');
                }
            });
        }
        
        async function handleRegistration() {
            if (!isAuthReady) {
                showMessageModal('사용자 인증 중입니다. 잠시 후 다시 시도해 주세요.');
                return;
            }

            const name = nameInput.value.trim();
            const isRegistered = userList.some(u => u.userId === userId);
            const scheduleDocRef = doc(db, publicSchedulesPath, tripId);

            try {
                if (isRegistered) {
                    const userToRemove = userList.find(u => u.userId === userId);
                    await updateDoc(scheduleDocRef, {
                        registeredUsers: arrayRemove(userToRemove),
                        assignments: JSON.parse(JSON.stringify(seatAssignments, (key, value) => {
                            if (value && value.userId === userId) return undefined;
                            return value;
                        }))
                    });
                    showMessageModal('신청이 취소되었습니다.');
                } else {
                    if (!name) {
                        showMessageModal('이름을 입력해 주세요.');
                        return;
                    }
                    await updateDoc(scheduleDocRef, {
                        registeredUsers: arrayUnion({ userId: userId, name: name })
                    });
                    showMessageModal('신청이 완료되었습니다.');
                }
            } catch (error) {
                console.error("Registration handling error:", error);
                showMessageModal('신청 처리 중 오류가 발생했습니다. 콘솔을 확인해주세요.');
            }
        }
        
        function updatePhotosAndTips(data) {
            mountainPhotoGallery.innerHTML = '';
            const mountainPhotos = data?.mountainPhotos || [];
            if (mountainPhotos.length > 0) {
                mountainPhotos.forEach(photoBase64 => {
                    mountainPhotoGallery.innerHTML += `
                        <div class="relative group">
                            <img src="${photoBase64}" class="w-full h-auto object-cover rounded-lg shadow-md" onerror="this.src='https://placehold.co/200x200?text=사진+오류'"/>
                            <button onclick="removeMountainPhoto('${photoBase64}')" class="absolute top-1 right-1 bg-red-600 text-white rounded-full p-1 opacity-0 group-hover:opacity-100 transition-opacity">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                                </svg>
                            </button>
                        </div>
                    `;
                });
            } else {
                mountainPhotoGallery.innerHTML = `<div class="col-span-full text-center text-gray-500">사진이 없습니다.</div>`;
            }

            hikingTipsContent.value = data?.hikingTips || '';

            personalPhotoGallery.innerHTML = '';
            const personalPhotos = data?.personalPhotos || [];
            if (personalPhotos.length > 0) {
                personalPhotos.forEach(photoBase64 => {
                    personalPhotoGallery.innerHTML += `
                        <div class="relative group">
                            <img src="${photoBase64}" class="w-full h-auto object-cover rounded-lg shadow-md" onerror="this.src='https://placehold.co/200x200?text=사진+오류'"/>
                            <button onclick="removePersonalPhoto('${photoBase64}')" class="absolute top-1 right-1 bg-red-600 text-white rounded-full p-1 opacity-0 group-hover:opacity-100 transition-opacity">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                                </svg>
                            </button>
                        </div>
                    `;
                });
            } else {
                personalPhotoGallery.innerHTML = `<div class="col-span-full text-center text-gray-500">사진이 없습니다.</div>`;
            }
        }

        async function addMountainPhoto(event) {
            if (!isAuthReady) {
                showMessageModal('사용자 인증 중입니다. 잠시 후 다시 시도해 주세요.');
                return;
            }
            const file = event.target.files[0];
            if (!file) return;
            if (!file.type.startsWith('image/')) {
                showMessageModal('이미지 파일만 업로드할 수 있습니다.');
                return;
            }
            const reader = new FileReader();
            reader.readAsDataURL(file);
            reader.onload = async () => {
                const photoBase64 = reader.result;
                try {
                    const scheduleDocRef = doc(db, publicSchedulesPath, tripId);
                    await updateDoc(scheduleDocRef, { mountainPhotos: arrayUnion(photoBase64) });
                    showMessageModal('사진이 성공적으로 추가되었습니다.');
                } catch (error) {
                    console.error("사진 추가 중 오류:", error);
                    showMessageModal('사진 추가 중 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.');
                }
            };
        }

        async function removeMountainPhoto(photoBase64) {
            if (!isAuthReady) {
                showMessageModal('사용자 인증 중입니다. 잠시 후 다시 시도해 주세요.');
                return;
            }
            try {
                const scheduleDocRef = doc(db, publicSchedulesPath, tripId);
                await updateDoc(scheduleDocRef, { mountainPhotos: arrayRemove(photoBase64) });
                showMessageModal('사진이 삭제되었습니다.');
            } catch (error) {
                console.error("사진 삭제 중 오류:", error);
                showMessageModal('사진 삭제 중 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.');
            }
        }

        async function saveHikingTips() {
            if (!isAuthReady) {
                showMessageModal('사용자 인증 중입니다. 잠시 후 다시 시도해 주세요.');
                return;
            }
            try {
                const scheduleDocRef = doc(db, publicSchedulesPath, tripId);
                await updateDoc(scheduleDocRef, { hikingTips: hikingTipsContent.value });
                showMessageModal('요령이 성공적으로 저장되었습니다.');
            } catch (error) {
                console.error("요령 저장 중 오류:", error);
                showMessageModal('요령 저장 중 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.');
            }
        }

        async function addPersonalPhoto(event) {
            if (!isAuthReady) {
                showMessageModal('사용자 인증 중입니다. 잠시 후 다시 시도해 주세요.');
                return;
            }
            const file = event.target.files[0];
            if (!file) return;
            if (!file.type.startsWith('image/')) {
                showMessageModal('이미지 파일만 업로드할 수 있습니다.');
                return;
            }
            const reader = new FileReader();
            reader.readAsDataURL(file);
            reader.onload = async () => {
                const photoBase64 = reader.result;
                try {
                    const scheduleDocRef = doc(db, publicSchedulesPath, tripId);
                    await updateDoc(scheduleDocRef, { personalPhotos: arrayUnion(photoBase64) });
                    showMessageModal('사진이 갤러리에 추가되었습니다.');
                } catch (error) {
                    console.error("개인 사진 추가 중 오류:", error);
                    showMessageModal('사진 추가 중 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.');
                }
            };
        }
        
        async function removePersonalPhoto(photoBase64) {
            if (!isAuthReady) {
                showMessageModal('사용자 인증 중입니다. 잠시 후 다시 시도해 주세요.');
                return;
            }
            try {
                const scheduleDocRef = doc(db, publicSchedulesPath, tripId);
                await updateDoc(scheduleDocRef, { personalPhotos: arrayRemove(photoBase64) });
                showMessageModal('사진이 삭제되었습니다.');
            } catch (error) {
                console.error("개인 사진 삭제 중 오류:", error);
                showMessageModal('사진 삭제 중 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.');
            }
        }

        function showView(viewName) {
            document.getElementById('scheduleSection').classList.add('hidden');
            document.getElementById('boardSection').classList.add('hidden');
            document.getElementById('menu-schedule').classList.remove('border-indigo-300', 'text-white');
            document.getElementById('menu-board').classList.remove('border-indigo-300', 'text-white');
            document.getElementById('menu-schedule').classList.add('border-transparent', 'text-indigo-200');
            document.getElementById('menu-board').classList.add('border-transparent', 'text-indigo-200');

            if (viewName === 'schedule') {
                document.getElementById('scheduleSection').classList.remove('hidden');
                document.getElementById('menu-schedule').classList.add('border-indigo-300', 'text-white');
                document.getElementById('menu-schedule').classList.remove('border-transparent', 'text-indigo-200');
            } else if (viewName === 'board') {
                document.getElementById('boardSection').classList.remove('hidden');
                document.getElementById('menu-board').classList.add('border-indigo-300', 'text-white');
                document.getElementById('menu-board').classList.remove('border-transparent', 'text-indigo-200');
                showBoardView('free');
            }
        }
        
        function showBoardView(viewName) {
            document.getElementById('freeBoardView').classList.add('hidden');
            document.getElementById('recommendBoardView').classList.add('hidden');
            document.getElementById('board-free').classList.remove('border-indigo-600', 'text-indigo-600');
            document.getElementById('board-recommend').classList.remove('border-indigo-600', 'text-indigo-600');
            document.getElementById('board-free').classList.add('border-transparent');
            document.getElementById('board-recommend').classList.add('border-transparent');

            if (viewName === 'free') {
                document.getElementById('freeBoardView').classList.remove('hidden');
                document.getElementById('board-free').classList.add('border-indigo-600', 'text-indigo-600');
                document.getElementById('board-free').classList.remove('border-transparent');
            } else if (viewName === 'recommend') {
                document.getElementById('recommendBoardView').classList.remove('hidden');
                document.getElementById('board-recommend').classList.add('border-indigo-600', 'text-indigo-600');
                document.getElementById('board-recommend').classList.remove('border-transparent');
            }
        }

        async function submitPost(type) {
            if (!isAuthReady) {
                showMessageModal('사용자 인증 중입니다. 잠시 후 다시 시도해 주세요.');
                return;
            }
            let content, title;
            let colRef;
            if (type === 'free') {
                content = document.getElementById('postContent').value.trim();
                if (!content) { showMessageModal('게시물 내용을 입력해 주세요.'); return; }
                colRef = collection(db, postsCollectionPath);
            } else if (type === 'recommend') {
                title = document.getElementById('recommendationTitle').value.trim();
                content = document.getElementById('recommendationContent').value.trim();
                if (!title || !content) { showMessageModal('제목과 내용을 모두 입력해 주세요.'); return; }
                colRef = collection(db, recommendationsCollectionPath);
            }
            try {
                await addDoc(colRef, { userId: userId, userName: userList.find(u => u.userId === userId)?.name || '익명', content: content, title: title, timestamp: Date.now() });
                if (type === 'free') { document.getElementById('postContent').value = ''; }
                else if (type === 'recommend') { document.getElementById('recommendationTitle').value = ''; document.getElementById('recommendationContent').value = ''; }
                showMessageModal('게시물이 성공적으로 등록되었습니다.');
            } catch (error) {
                console.error("게시물 제출 오류:", error);
                showMessageModal('게시물 제출 중 오류가 발생했습니다. 콘솔을 확인해주세요.');
            }
        }

        function renderPosts(posts, container, isRecommendation = false) {
            if (posts.length === 0) {
                container.innerHTML = `<div class="text-gray-500 text-center">아직 게시물이 없습니다.</div>`;
                return;
            }
            container.innerHTML = posts.map(post => {
                let postTitle = isRecommendation ? `<p class="text-sm font-bold text-gray-800 mb-1">제목: ${post.title}</p>` : '';
                return `
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-gray-200">
                        <p class="text-xs font-bold text-gray-800 mb-1">작성자: ${post.userName}</p>
                        ${postTitle}
                        <p class="text-gray-700">${post.content}</p>
                        <p class="text-xs text-gray-400 mt-2">${new Date(post.timestamp).toLocaleString()}</p>
                    </div>
                `;
            }).join('');
        }
        
        function showMessageModal(message) {
            const modal = document.getElementById('messageModal');
            document.getElementById('modalMessage').textContent = message;
            modal.classList.remove('hidden');
        }

        window.onload = initFirebase;
        window.handleRegistration = handleRegistration;
        window.saveTripInfo = saveTripInfo;
        window.addMountainPhoto = addMountainPhoto;
        window.removeMountainPhoto = removeMountainPhoto;
        window.saveHikingTips = saveHikingTips;
        window.addPersonalPhoto = addPersonalPhoto;
        window.removePersonalPhoto = removePersonalPhoto;
        window.showView = showView;
        window.showBoardView = showBoardView;
        window.submitPost = submitPost;
    </script>
</body>
</html>

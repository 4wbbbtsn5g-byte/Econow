# Econow<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Plastic Finder</title>

<style>
    body {
        font-family: 'Arial', sans-serif;
        background: #f2f7fb;
        margin: 0;
        padding: 0;
        text-align: center;
    }

    header {
        padding: 40px 10px;
        background: #ffffff;
        box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }

    h1 {
        margin-bottom: 10px;
        font-size: 32px;
        color: #2c4d6d;
    }

    .section {
        margin: 30px auto;
        width: 90%;
        max-width: 600px;
        background: #fff;
        padding: 20px;
        border-radius: 15px;
        box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }

    .option-title {
        font-size: 20px;
        margin-bottom: 10px;
        font-weight: bold;
        color: #2c4d6d;
    }

    .btn-group {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 10px;
        margin-bottom: 20px;
    }

    button {
        padding: 10px 16px;
        border: none;
        border-radius: 8px;
        background: #d9e6f2;
        color: #2c4d6d;
        font-size: 16px;
        cursor: pointer;
    }

    button.active {
        background: #2c4d6d;
        color: white;
        font-weight: bold;
    }

    #result {
        margin-top: 20px;
        padding: 20px;
        border-radius: 15px;
        background: #e9f2fa;
        display: none;
        text-align: left;
    }

    .result-title {
        font-size: 22px;
        font-weight: bold;
        margin-bottom: 10px;
        color: #2c4d6d;
    }

    .icon {
        font-size: 50px;
        margin-bottom: 10px;
    }
</style>
</head>

<body>

<header>
    <h1>Plastic Finder : 플라스틱 종류 분류 도우미</h1>
    <p>색 · 질감 · 두께를 선택하면 플라스틱 종류를 알려드립니다.</p>
</header>

<!-- 선택 영역 -->
<div class="section">
    <div class="option-title">① 색</div>
    <div class="btn-group" data-type="color">
        <button>투명</button>
        <button>불투명</button>
        <button>반투명</button>
    </div>

    <div class="option-title">② 질감</div>
    <div class="btn-group" data-type="texture">
        <button>딱딱함</button>
        <button>부드러움</button>
        <button>잘 구겨짐</button>
    </div>

    <div class="option-title">③ 두께</div>
    <div class="btn-group" data-type="thickness">
        <button>얇음</button>
        <button>중간</button>
        <button>두꺼움</button>
    </div>

    <div class="option-title">④ 특징</div>
    <div class="btn-group" data-type="feature">
        <button>라벨 있음</button>
        <button>라벨 없음</button>
        <button>탄성 있음</button>
        <button>탄성 없음</button>
    </div>
</div>

<!-- 결과 영역 -->
<div class="section" id="result">
    <div class="icon" id="icon">🔍</div>
    <div class="result-title" id="plastic-type"></div>
    <div id="plastic-desc"></div>
    <br />
    <div id="recycle-tip"></div>
</div>

<script>
    const selections = {
        color: null,
        texture: null,
        thickness: null,
        feature: null
    };

    const PLASTIC_DATA = {
        PET: {
            icon: "🥤",
            desc: "투명하고 단단하며 라벨이 붙은 경우가 많습니다.",
            tip: "라벨 제거 후 찌그러뜨려 배출하면 재활용 효율이 증가합니다."
        },
        PP: {
            icon: "🍱",
            desc: "내열성이 좋고 단단한 식품용기에 주로 쓰입니다.",
            tip: "기름기를 깨끗이 씻어 배출하세요."
        },
        PS: {
            icon: "🧁",
            desc: "흰색 트레이나 일회용 용기에 흔히 사용됩니다.",
            tip: "깨지기 쉬우니 분리배출 시 주의하세요."
        },
        LDPE: {
            icon: "🛍️",
            desc: "비닐봉투처럼 얇고 잘 구겨지는 재질입니다.",
            tip: "물기 제거 후 가벼운 비닐류만 분리배출하세요."
        }
    };

    function calculateResult() {
        const { color, texture, thickness, feature } = selections;

        if (!color || !texture || !thickness || !feature) return;

        let result = "PET"; // 기본값

        if (color === "투명" && texture === "딱딱함") result = "PET";
        else if (texture === "부드러움" && thickness === "중간") result = "PP";
        else if (thickness === "두꺼움") result = "PS";
        else if (texture === "잘 구겨짐" || thickness === "얇음") result = "LDPE";

        displayResult(result);
    }

    function displayResult(type) {
        const data = PLASTIC_DATA[type];

        document.getElementById("result").style.display = "block";
        document.getElementById("icon").innerText = data.icon;
        document.getElementById("plastic-type").innerText = `추정 플라스틱 종류 : ${type}`;
        document.getElementById("plastic-desc").innerText = data.desc;
        document.getElementById("recycle-tip").innerText = "✅ 재활용 팁: " + data.tip;
    }

    document.querySelectorAll(".btn-group button").forEach(btn => {
        btn.addEventListener("click", function () {
            const group = this.parentElement.getAttribute("data-type");

            // 선택상태 변경
            [...this.parentElement.children].forEach(b => b.classList.remove("active"));
            this.classList.add("active");

            selections[group] = this.innerText;

            calculateResult();
        });
    });
</script>

</body>
</html>

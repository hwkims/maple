메이플스토리의 API를 이용하여 웹 서비스를 만드는 방법에 대해 설명하겠습니다. 이 서비스는 메이플스토리 게임의 캐릭터 데이터를 조회하는 기능을 제공할 것입니다. 메이플스토리 API를 호출하여 캐릭터 정보를 조회하고, 그 결과를 사용자에게 보여주는 웹 애플리케이션을 만들 것입니다.

아래는 HTML, CSS, JavaScript를 사용하여 메이플스토리 API를 활용한 웹 서비스 코드입니다. 사용자는 캐릭터 식별자(OCID)를 입력하고, 다양한 캐릭터 정보를 조회할 수 있습니다.

---

### **메이플스토리 캐릭터 정보 웹 서비스 예시**

#### **1. HTML 코드 (UI 설계)**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>메이플스토리 캐릭터 정보</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: linear-gradient(45deg, rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.5));
            color: white;
        }

        .container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(15px);
            border-radius: 15px;
            padding: 30px;
            width: 90%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            font-size: 2rem;
            margin-bottom: 20px;
        }

        .input-group {
            margin: 10px 0;
        }

        .input-group input {
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ddd;
            width: 100%;
            margin-top: 5px;
            color: black;
        }

        .input-group button {
            padding: 12px 20px;
            border: none;
            background-color: #4CAF50;
            color: white;
            font-size: 16px;
            cursor: pointer;
            border-radius: 5px;
            margin-top: 20px;
        }

        .input-group button:hover {
            background-color: #45a049;
        }

        .loading {
            display: none;
            color: #ff9800;
        }

        .result {
            margin-top: 20px;
            font-size: 1.2rem;
            color: #4CAF50;
        }

        .error {
            color: #ff0000;
        }

        .info {
            text-align: left;
            margin-top: 20px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>메이플스토리 캐릭터 정보 조회</h1>

        <div class="input-group">
            <label for="characterId">캐릭터 식별자(OCID):</label>
            <input type="text" id="characterId" placeholder="캐릭터 ID를 입력하세요">
        </div>

        <button onclick="getCharacterInfo()">캐릭터 정보 조회</button>

        <div class="loading" id="loading">데이터를 불러오는 중...</div>
        <div class="result" id="result"></div>
        <div class="error" id="error"></div>
    </div>

    <script>
        const apiKey = 'test_b3f846bd41a2b72ca7fe1ca7beff943908357d43678b1fe9e8d522c28c07a506efe8d04e6d233bd35cf2fabdeb93fb0d'; // API 키 입력

        async function getCharacterInfo() {
            const characterId = document.getElementById('characterId').value.trim();
            if (!characterId) {
                alert("캐릭터 ID를 입력하세요.");
                return;
            }

            document.getElementById('loading').style.display = 'block';
            document.getElementById('result').innerHTML = '';
            document.getElementById('error').innerHTML = '';

            try {
                const response = await fetch(`https://open.api.nexon.com/maplestory/v1/character/basic?ocid=${characterId}`, {
                    method: 'GET',
                    headers: {
                        'Authorization': `Bearer ${apiKey}`
                    }
                });

                if (!response.ok) {
                    throw new Error('API 호출 오류');
                }

                const data = await response.json();
                document.getElementById('loading').style.display = 'none';
                document.getElementById('result').innerHTML = `
                    <div class="info">
                        <h3>캐릭터 기본 정보:</h3>
                        <p><strong>캐릭터명:</strong> ${data.characterName}</p>
                        <p><strong>레벨:</strong> ${data.level}</p>
                        <p><strong>직업:</strong> ${data.job}</p>
                        <p><strong>성별:</strong> ${data.gender}</p>
                        <p><strong>길드명:</strong> ${data.guild}</p>
                    </div>
                `;
            } catch (error) {
                document.getElementById('loading').style.display = 'none';
                document.getElementById('error').innerHTML = "캐릭터 정보를 불러오는 데 실패했습니다. 캐릭터 ID를 확인하세요.";
            }
        }
    </script>

</body>
</html>
```

---

### **설명**:

1. **HTML 구조**:
   - 사용자는 **캐릭터 식별자(OCID)**를 입력하고, **"캐릭터 정보 조회"** 버튼을 클릭하여 메이플스토리 캐릭터의 기본 정보를 조회할 수 있습니다.

2. **API 호출**:
   - `getCharacterInfo()` 함수는 캐릭터 ID를 기반으로 메이플스토리 API의 `/v1/character/basic` 엔드포인트에 요청을 보내어 해당 캐릭터의 기본 정보를 받아옵니다.
   - API 응답 데이터에서 `characterName`, `level`, `job`, `gender`, `guild` 등의 정보를 출력합니다.

3. **에러 처리**:
   - 캐릭터 ID가 비어있거나 API 호출에 실패할 경우, 적절한 오류 메시지가 표시됩니다.

4. **디자인**:
   - **글래스모픽(Glassmorphism)** 효과를 적용하여, 페이지 배경과 콘텐츠의 배경이 흐리게 처리됩니다.
   - 기본적인 **반응형 디자인**을 사용하여 모바일에서도 잘 동작합니다.

---

### **API 키 및 엔드포인트**:
- **API 키**: `test_b3f846bd41a2b72ca7fe1ca7beff943908357d43678b1fe9e8d522c28c07a506efe8d04e6d233bd35cf2fabdeb93fb0d`
- **엔드포인트**: `https://open.api.nexon.com/maplestory/v1/character/basic?ocid={캐릭터ID}`

### **기능 확장**:
- **캐릭터 목록 조회**: `/maplestory/v1/character/list` 엔드포인트를 사용하여 해당 사용자의 캐릭터 목록을 조회할 수 있습니다.
- **기타 데이터**: `character/popularity`, `character/stat`와 같은 다른 엔드포인트를 추가하여, 다양한 캐릭터 정보를 조회할 수 있습니다.

---

이 코드는 메이플스토리 API를 활용한 기본적인 캐릭터 정보 조회 서비스를 구현한 예제입니다. API 키를 제공하고, 사용자가 캐릭터 ID를 입력하면 관련 정보를 불러올 수 있습니다. 추가적인 기능이나 다른 데이터를 조회하려면 API 문서를 참고하여 원하는 데이터를 더 쉽게 확장할 수 있습니다.

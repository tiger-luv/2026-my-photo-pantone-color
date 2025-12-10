const imgInput = document.getElementById("imgInput");
const nameInput = document.getElementById("nameInput");
const makeBtn = document.getElementById("makeBtn");
const canvas = document.getElementById("resultCanvas");
const ctx = canvas.getContext("2d");

// 임시 팬톤 컬러 목록 (필요시 확장 가능)
const pantoneList = [
  { name: "PANTONE 186 C", r: 200, g: 16, b: 46 },
  { name: "PANTONE 300 C", r: 0, g: 91, b: 187 },
  { name: "PANTONE 7406 C", r: 245, g: 200, b: 0 },
  { name: "PANTONE 347 C", r: 0, g: 155, b: 72 },
  { name: "PANTONE 7672 C", r: 95, g: 91, b: 160 }
];

// 색 차이 계산
function colorDistance(c1, c2) {
  return Math.sqrt(
    (c1.r - c2.r) ** 2 +
    (c1.g - c2.g) ** 2 +
    (c1.b - c2.b) ** 2
  );
}

// 사진에서 평균 색 추출
function getAverageColor(img, w, h) {
  const tCanvas = document.createElement("canvas");
  const tCtx = tCanvas.getContext("2d");

  tCanvas.width = w;
  tCanvas.height = h;
  tCtx.drawImage(img, 0, 0, w, h);

  const data = tCtx.getImageData(0, 0, w, h).data;

  let r = 0, g = 0, b = 0;
  let count = 0;

  for (let i = 0; i < data.length; i += 4) {
    r += data[i];
    g += data[i + 1];
    b += data[i + 2];
    count++;
  }

  return { r: r / count, g: g / count, b: b / count };
}

// 팬톤 매칭
function findPantoneColor(color) {
  let best = null;
  let bestDist = Infinity;

  pantoneList.forEach(p => {
    const dist = colorDistance(color, p);
    if (dist < bestDist) {
      bestDist = dist;
      best = p;
    }
  });

  return best;
}

// ★ 완성하기 버튼 클릭
makeBtn.addEventListener("click", () => {
  const file = imgInput.files[0];
  if (!file) return alert("사진을 선택해주세요!");

  const img = new Image();
  img.src = URL.createObjectURL(file);

  img.onload = () => {
    const iw = img.width;
    const ih = img.height;

    canvas.width = iw;
    canvas.height = ih;

    ctx.drawImage(img, 0, 0);

    // 사진의 평균색 추출 → 팬톤 매칭
    const avgColor = getAverageColor(img, 30, 30);
    const pantone = findPantoneColor(avgColor);

    // 텍스트 기본 설정
    const padding = iw * 0.03;
    const yearFont = iw * 0.07; // 크게 복원
    const nameFont = iw * 0.10; // 크게 복원

    ctx.fillStyle = "#FFFFFF";
    ctx.textBaseline = "bottom";

    const nameText = nameInput.value || "OO OOO의 색";
    const txtH = yearFont + nameFont;

    // 왼쪽 하단 큰 글씨 2줄
    ctx.font = `${yearFont}px sans-serif`;
    ctx.fillText("2026년 올해의 컬러", padding, ih - padding - nameFont - 10);

    ctx.font = `${nameFont}px sans-serif`;
    ctx.fillText(nameText, padding, ih - padding);

    // 오른쪽 중앙 팬톤 박스
    const boxW = iw * 0.25;
    const boxH = iw * 0.25;

    const boxX = iw - boxW - padding;
    const boxY = ih / 2 - boxH / 2;

    ctx.fillStyle = `rgb(${pantone.r}, ${pantone.g}, ${pantone.b})`;
    ctx.fillRect(boxX, boxY, boxW, boxH);

    ctx.fillStyle = "#fff";
    ctx.font = `${iw * 0.035}px sans-serif`;
    ctx.fillText(pantone.name, boxX + 10, boxY + boxH - 20);

    canvas.style.display = "block";
  };
});

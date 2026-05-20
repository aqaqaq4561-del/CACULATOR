<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>기사 정산 계산기 예시</title>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Pretendard, sans-serif;
      background: #f4f6f8;
      color: #1f2937;
    }

    .page {
      max-width: 1080px;
      margin: 0 auto;
      padding: 32px 20px 56px;
    }

    .header {
      margin-bottom: 24px;
    }

    .badge {
      display: inline-flex;
      align-items: center;
      padding: 6px 12px;
      border-radius: 999px;
      background: #e8eefc;
      color: #3156b7;
      font-size: 13px;
      font-weight: 700;
      margin-bottom: 12px;
    }

    h1 {
      margin: 0 0 8px;
      font-size: 30px;
      letter-spacing: -0.04em;
    }

    .desc {
      margin: 0;
      color: #6b7280;
      line-height: 1.6;
      font-size: 15px;
    }

    .layout {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }

    .card {
      background: #fff;
      border: 1px solid #e5e7eb;
      border-radius: 20px;
      box-shadow: 0 10px 24px rgba(15, 23, 42, 0.06);
      overflow: hidden;
    }

    .card-header {
      padding: 20px 22px 14px;
      border-bottom: 1px solid #eef0f3;
    }

    .card-title {
      margin: 0;
      font-size: 18px;
      font-weight: 800;
      letter-spacing: -0.03em;
    }

    .card-subtitle {
      margin: 6px 0 0;
      color: #6b7280;
      font-size: 13px;
    }

    .card-body {
      padding: 22px;
    }

    .form-grid {
      display: grid;
      gap: 16px;
    }

    .field label {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 14px;
      font-weight: 700;
      margin-bottom: 8px;
    }

    .hint {
      font-size: 12px;
      color: #9ca3af;
      font-weight: 500;
    }

    .input-wrap {
      position: relative;
    }

    input {
      width: 100%;
      height: 48px;
      border: 1px solid #d9dee8;
      border-radius: 14px;
      padding: 0 44px 0 14px;
      font-size: 16px;
      outline: none;
      transition: 0.15s;
      background: #fbfcfe;
    }

    input:focus {
      border-color: #3156b7;
      background: #fff;
      box-shadow: 0 0 0 4px rgba(49, 86, 183, 0.1);
    }

    .unit {
      position: absolute;
      right: 14px;
      top: 50%;
      transform: translateY(-50%);
      color: #6b7280;
      font-size: 14px;
      font-weight: 700;
    }

    .notice {
      margin-top: 16px;
      padding: 14px 16px;
      border-radius: 14px;
      background: #f8fafc;
      border: 1px dashed #cbd5e1;
      color: #64748b;
      font-size: 13px;
      line-height: 1.55;
    }

    .summary-box {
      border-radius: 18px;
      padding: 20px;
      margin-bottom: 18px;
      background: linear-gradient(135deg, #3156b7, #1e3a8a);
      color: #fff;
    }

    .summary-label {
      font-size: 14px;
      opacity: 0.82;
      margin-bottom: 8px;
    }

    .summary-value {
      font-size: 32px;
      font-weight: 900;
      letter-spacing: -0.04em;
      margin-bottom: 8px;
    }

    .summary-text {
      font-size: 14px;
      line-height: 1.6;
      opacity: 0.9;
    }

    .summary-box.negative {
      background: linear-gradient(135deg, #dc2626, #991b1b);
    }

    .summary-box.zero {
      background: linear-gradient(135deg, #475569, #1f2937);
    }

    .row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      padding: 14px 0;
      border-bottom: 1px solid #eef0f3;
      font-size: 14px;
    }

    .row:last-child {
      border-bottom: 0;
    }

    .row strong {
      font-size: 15px;
    }

    .row .name {
      color: #4b5563;
    }

    .row .value {
      font-weight: 800;
      text-align: right;
    }

    .divider-title {
      margin: 22px 0 8px;
      font-size: 14px;
      font-weight: 900;
      color: #111827;
    }

    .status-pill {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      padding: 8px 12px;
      border-radius: 999px;
      font-size: 13px;
      font-weight: 800;
      background: #eef2ff;
      color: #3156b7;
      margin-top: 10px;
    }

    .status-pill.negative {
      background: #fee2e2;
      color: #b91c1c;
    }

    .status-pill.zero {
      background: #e5e7eb;
      color: #374151;
    }

    .formula {
      margin-top: 18px;
      padding: 16px;
      border-radius: 14px;
      background: #f9fafb;
      border: 1px solid #e5e7eb;
      font-size: 13px;
      color: #4b5563;
      line-height: 1.7;
    }

    .formula b {
      color: #111827;
    }

    .error {
      margin-top: 12px;
      display: none;
      color: #dc2626;
      font-size: 13px;
      line-height: 1.5;
    }

    .error.show {
      display: block;
    }

    @media (max-width: 860px) {
      .layout {
        grid-template-columns: 1fr;
      }

      h1 {
        font-size: 25px;
      }

      .summary-value {
        font-size: 27px;
      }
    }
  </style>
</head>
<body>
  <main class="page">
    <section class="header">
      <div class="badge">정산 계산기 화면 예시</div>
      <h1>기사 정산 자동 계산</h1>
      <p class="desc">
        고객이 낸 총 금액에서 부품비를 빼고, 기사 수수료율을 적용한 뒤,<br />
        기사가 현장에서 이미 받은 현금을 차감하여 최종 지급/회수 금액을 계산합니다.
      </p>
    </section>

    <section class="layout">
      <div class="card">
        <div class="card-header">
          <h2 class="card-title">작업 완료 금액 입력</h2>
          <p class="card-subtitle">기사 앱 또는 관리자 페이지에서 입력하는 영역</p>
        </div>
        <div class="card-body">
          <div class="form-grid">
            <div class="field">
              <label>총 고객 결제금액 <span class="hint">직접 입력</span></label>
              <div class="input-wrap">
                <input id="totalAmount" type="text" inputmode="numeric" value="3,000,000" />
                <span class="unit">원</span>
              </div>
            </div>

            <div class="field">
              <label>기사 현금 수령액 <span class="hint">기사가 이미 받은 돈</span></label>
              <div class="input-wrap">
                <input id="cashReceived" type="text" inputmode="numeric" value="500,000" />
                <span class="unit">원</span>
              </div>
            </div>

            <div class="field">
              <label>카드 결제금액 <span class="hint">회사로 들어올 돈</span></label>
              <div class="input-wrap">
                <input id="cardAmount" type="text" inputmode="numeric" value="1,000,000" />
                <span class="unit">원</span>
              </div>
            </div>

            <div class="field">
              <label>계산서/계좌입금 금액 <span class="hint">회사로 들어올 돈</span></label>
              <div class="input-wrap">
                <input id="invoiceAmount" type="text" inputmode="numeric" value="1,500,000" />
                <span class="unit">원</span>
              </div>
            </div>

            <div class="field">
              <label>부품비 <span class="hint">정산 전에 먼저 차감</span></label>
              <div class="input-wrap">
                <input id="partsCost" type="text" inputmode="numeric" value="0" />
                <span class="unit">원</span>
              </div>
            </div>

            <div class="field">
              <label>기사 수수료율 <span class="hint">관리자가 수정 가능</span></label>
              <div class="input-wrap">
                <input id="commissionRate" type="text" inputmode="decimal" value="50" />
                <span class="unit">%</span>
              </div>
            </div>
          </div>

          <div id="error" class="error"></div>

          <div class="notice">
            예시: 총 고객 결제금액을 300만원으로 직접 입력하고, 그중 기사가 현금 50만원을 이미 받았으며 기사 몫이 50%라면 기사 총 정산금은 150만원입니다. 이미 받은 50만원을 빼고 회사가 기사에게 100만원을 지급합니다.
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-header">
          <h2 class="card-title">자동 계산 결과</h2>
          <p class="card-subtitle">관리자 정산 상세 화면에 보여줄 영역</p>
        </div>
        <div class="card-body">
          <div id="summaryBox" class="summary-box">
            <div class="summary-label">최종 정산 결과</div>
            <div id="summaryValue" class="summary-value">0원</div>
            <div id="summaryText" class="summary-text">계산 중입니다.</div>
            <div id="statusPill" class="status-pill">지급대기</div>
          </div>

          <div class="row">
            <span class="name">정산 대상 금액</span>
            <strong id="settlementBase" class="value">0원</strong>
          </div>
          <div class="row">
            <span class="name">기사 총 정산금</span>
            <strong id="workerSettlement" class="value">0원</strong>
          </div>
          <div class="row">
            <span class="name">기사 현금 수령액</span>
            <strong id="cashAlready" class="value">0원</strong>
          </div>
          <div class="row">
            <span class="name">회사 수령 예정액</span>
            <strong id="companyReceived" class="value">0원</strong>
          </div>

          <div class="divider-title">회사 기준 최종 금액</div>
          <div class="row">
            <span class="name">회사 최종 몫</span>
            <strong id="companyFinalShare" class="value">0원</strong>
          </div>
          <div class="row">
            <span class="name">부품비 회수분</span>
            <strong id="partsRecovery" class="value">0원</strong>
          </div>

          <div class="formula">
            <b>계산식</b><br />
            정산 대상 금액 = 총 고객 결제금액 - 부품비<br />
            기사 총 정산금 = 정산 대상 금액 × 기사 수수료율<br />
            최종 지급/회수 금액 = 기사 총 정산금 - 기사 현금 수령액
          </div>
        </div>
      </div>
    </section>
  </main>

  <script>
    const inputs = [
      "totalAmount",
      "cashReceived",
      "cardAmount",
      "invoiceAmount",
      "partsCost",
      "commissionRate",
    ];

    const $ = (id) => document.getElementById(id);

    const formatWon = (value) => {
      const rounded = Math.round(value);
      return rounded.toLocaleString("ko-KR") + "원";
    };

    const parseNumber = (value) => {
      const cleaned = String(value || "")
        .replace(/,/g, "")
        .replace(/원/g, "")
        .replace(/%/g, "")
        .trim();
      const number = Number(cleaned);
      return Number.isFinite(number) ? number : 0;
    };

    const getNumber = (id) => parseNumber($(id).value);

    const formatInputWon = (id) => {
      const value = getNumber(id);
      $(id).value = Math.round(value).toLocaleString("ko-KR");
    };

    const normalizeRate = () => {
      const value = getNumber("commissionRate");
      $("commissionRate").value = String(value);
    };

    function calculate() {
      const totalAmount = getNumber("totalAmount");
      const cashReceived = getNumber("cashReceived");
      const cardAmount = getNumber("cardAmount");
      const invoiceAmount = getNumber("invoiceAmount");
      const partsCost = getNumber("partsCost");
      const commissionRate = getNumber("commissionRate");

      const paymentSum = cashReceived + cardAmount + invoiceAmount;
      const settlementBase = Math.max(totalAmount - partsCost, 0);
      const workerSettlement = settlementBase * (commissionRate / 100);
      const finalPayment = workerSettlement - cashReceived;
      const companyExpectedReceived = cardAmount + invoiceAmount;
      const companyFinalShare = settlementBase - workerSettlement;

      $("settlementBase").textContent = formatWon(settlementBase);
      $("workerSettlement").textContent = formatWon(workerSettlement);
      $("cashAlready").textContent = formatWon(cashReceived);
      $("companyReceived").textContent = formatWon(companyExpectedReceived);
      $("companyFinalShare").textContent = formatWon(companyFinalShare);
      $("partsRecovery").textContent = formatWon(partsCost);

      const summaryBox = $("summaryBox");
      const statusPill = $("statusPill");
      const summaryValue = $("summaryValue");
      const summaryText = $("summaryText");

      summaryBox.classList.remove("negative", "zero");
      statusPill.classList.remove("negative", "zero");

      if (finalPayment > 0) {
        summaryValue.textContent = formatWon(finalPayment);
        summaryText.textContent = "회사가 기사에게 추가로 지급해야 하는 금액입니다.";
        statusPill.textContent = "지급대기";
      } else if (finalPayment < 0) {
        summaryBox.classList.add("negative");
        statusPill.classList.add("negative");
        summaryValue.textContent = formatWon(Math.abs(finalPayment));
        summaryText.textContent = "기사가 회사에게 입금해야 하는 금액입니다.";
        statusPill.textContent = "입금요청";
      } else {
        summaryBox.classList.add("zero");
        statusPill.classList.add("zero");
        summaryValue.textContent = "0원";
        summaryText.textContent = "서로 추가로 주고받을 금액이 없습니다.";
        statusPill.textContent = "정산완료 가능";
      }

      const error = $("error");
      const messages = [];

      if (paymentSum !== totalAmount) {
        messages.push(
          `입력 확인: 현금+카드+계산서/계좌 합계는 ${formatWon(paymentSum)}인데, 총 고객 결제금액은 ${formatWon(totalAmount)}입니다.`
        );
      }

      if (partsCost > totalAmount) {
        messages.push("부품비가 총 고객 결제금액보다 큽니다. 금액을 확인해 주세요.");
      }

      if (commissionRate < 0 || commissionRate > 100) {
        messages.push("기사 수수료율은 0%~100% 사이로 입력해 주세요.");
      }

      if (messages.length > 0) {
        error.innerHTML = messages.join("<br />");
        error.classList.add("show");
      } else {
        error.innerHTML = "";
        error.classList.remove("show");
      }
    }

    inputs.forEach((id) => {
      $(id).addEventListener("input", calculate);
    });

    ["totalAmount", "cashReceived", "cardAmount", "invoiceAmount", "partsCost"].forEach((id) => {
      $(id).addEventListener("blur", () => {
        formatInputWon(id);
        calculate();
      });
    });

    $("commissionRate").addEventListener("blur", () => {
      normalizeRate();
      calculate();
    });

    calculate();
  </script>
</body>
</html>

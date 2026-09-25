<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>年会費返金シミュレーター</title>
<style>
  :root {
    --bg: #faf7f2;
    --card: #ffffff;
    --text: #2b2420;
    --sub: #7a6f63;
    --accent: #c9435f;
    --accent-soft: #f7e3e7;
    --border: #e8e0d6;
    box-sizing: border-box;
    padding-top: env(safe-area-inset-top, 0px);
    padding-bottom: env(safe-area-inset-bottom, 0px);
  }
  @media (prefers-color-scheme: dark) {
    :root:not([data-theme="light"]) {
      --bg: #1c1815;
      --card: #262019;
      --text: #f2ece3;
      --sub: #b3a89a;
      --accent: #e8748e;
      --accent-soft: #3a2529;
      --border: #3a332b;
    }
  }
  :root[data-theme="dark"] {
    --bg: #1c1815;
    --card: #262019;
    --text: #f2ece3;
    --sub: #b3a89a;
    --accent: #e8748e;
    --accent-soft: #3a2529;
    --border: #3a332b;
  }
  html, body { height: 100%; }
  * { box-sizing: border-box; }
  body {
    margin: 0;
    background: var(--bg);
    color: var(--text);
    font-family: "Hiragino Sans", "Noto Sans JP", -apple-system, BlinkMacSystemFont, sans-serif;
    display: flex;
    justify-content: center;
    padding: 24px 16px 40px;
  }
  .wrap { width: 100%; max-width: 420px; }
  h1 {
    font-size: 19px;
    font-weight: 700;
    margin: 0 0 6px;
    line-height: 1.4;
  }
  .lead {
    font-size: 13px;
    color: var(--sub);
    margin: 0 0 20px;
    line-height: 1.6;
  }
  .card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 20px;
    margin-bottom: 16px;
  }
  label {
    display: block;
    font-size: 13px;
    font-weight: 600;
    color: var(--sub);
    margin-bottom: 8px;
  }
  select {
    width: 100%;
    font-size: 17px;
    padding: 12px 14px;
    border-radius: 10px;
    border: 1px solid var(--border);
    background: var(--bg);
    color: var(--text);
    appearance: none;
    -webkit-appearance: none;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='9'%3E%3Cpath d='M1 1l6 6 6-6' stroke='%237a6f63' stroke-width='2' fill='none' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: right 14px center;
  }
  .result {
    text-align: center;
    padding: 22px 16px;
  }
  .result .amount {
    font-size: 38px;
    font-weight: 800;
    color: var(--accent);
    letter-spacing: -0.02em;
  }
  .result .amount span { font-size: 18px; font-weight: 700; margin-left: 2px; }
  .result .caption {
    font-size: 13px;
    color: var(--sub);
    margin-top: 6px;
  }
  .breakdown {
    margin-top: 14px;
    padding-top: 14px;
    border-top: 1px dashed var(--border);
    display: flex;
    justify-content: space-between;
    font-size: 13px;
    color: var(--sub);
  }
  .breakdown b { color: var(--text); }
  .note {
    background: var(--accent-soft);
    border-radius: 12px;
    padding: 14px 16px;
    font-size: 12.5px;
    color: var(--text);
    line-height: 1.7;
  }
  .note b { color: var(--accent); }
</style>
</head>
<body>
<div class="wrap">
  <h1>年会費 返金シミュレーター</h1>
  <p class="lead">FC閉鎖(9月末)にともなう年会費4,000円の月割返金額を、入会(更新)月から概算します。</p>

  <div class="card">
    <label for="month">次の更新月はいつですか?(＝本来なら年会費が発生していたはずの月)</label>
    <select id="month"></select>

    <div id="prepaidRow" style="display:none; margin-top:16px; padding-top:16px; border-top:1px dashed var(--border);">
      <label style="display:flex; align-items:flex-start; gap:10px; cursor:pointer; margin-bottom:0;">
        <input type="checkbox" id="prepaid" style="width:18px; height:18px; margin-top:2px; accent-color:var(--accent);">
        <span style="font-weight:400; color:var(--text); font-size:13.5px; line-height:1.6;">来年度分の年会費(4,000円)もすでに支払い済み</span>
      </label>
    </div>
  </div>

  <div class="card result">
    <div class="amount" id="amount">-</div>
    <div class="caption" id="caption"></div>
    <div class="breakdown">
      <div>未経過月数 <b id="months">-</b></div>
      <div>月あたり <b id="perMonth">-</b></div>
    </div>
    <div id="prepaidBreakdown" style="display:none; margin-top:10px; padding-top:10px; border-top:1px dashed var(--border); font-size:13px; color:var(--sub); text-align:left;">
      <div style="display:flex; justify-content:space-between;"><span>現契約の未経過分</span><b style="color:var(--text)" id="baseRefund">-</b></div>
      <div style="display:flex; justify-content:space-between; margin-top:4px;"><span>来年度分(全額)</span><b style="color:var(--text)" id="prepaidRefund">-</b></div>
    </div>
  </div>

  <div class="note">
    <b>計算の考え方:</b> 年会費4,000円は12か月分。「次の更新月(本来なら年会費が発生していたはずの月)」を基準に、9月末で契約がどこまで消化済みかを数え、10月以降に残る月数ぶんだけ4,000円÷12か月で月割り計算しています(1円未満切り捨て)。実際の返金額は運営の公式発表(端数処理や規約)により異なる場合があるため、目安としてご利用ください。
  </div>
</div>

<script>
  const ANNUAL_FEE = 4000;
  const monthSelect = document.getElementById('month');
  const prepaidRow = document.getElementById('prepaidRow');
  const prepaidCheckbox = document.getElementById('prepaid');
  const prepaidBreakdown = document.getElementById('prepaidBreakdown');
  const monthNames = ['1月','2月','3月','4月','5月','6月','7月','8月','9月','10月','11月','12月'];

  monthNames.forEach((name, i) => {
    const opt = document.createElement('option');
    opt.value = i + 1;
    opt.textContent = name;
    monthSelect.appendChild(opt);
  });
  monthSelect.value = 1; // デフォルトは1月

  function calc(renewalMonth) {
    // renewalMonth = 本来なら年会費が発生していたはずの月(=現契約の最終月)
    // 9月なら現契約はちょうど9月末で満了 → 未経過0か月
    const remaining = (renewalMonth - 9 + 12) % 12;
    const perMonth = Math.floor(ANNUAL_FEE / 12); // 333円
    const refund = perMonth * remaining;
    return { remaining, perMonth, refund };
  }

  function render() {
    const m = parseInt(monthSelect.value, 10);
    const { remaining, perMonth, refund } = calc(m);

    // 9月・10月期限の人だけ「来年度分すでに支払い済み」チェックを表示
    const canHavePrepaid = (m === 9 || m === 10);
    prepaidRow.style.display = canHavePrepaid ? 'block' : 'none';
    if (!canHavePrepaid) prepaidCheckbox.checked = false;

    const isPrepaid = canHavePrepaid && prepaidCheckbox.checked;
    const total = isPrepaid ? refund + ANNUAL_FEE : refund;

    document.getElementById('amount').innerHTML = total.toLocaleString() + '<span>円</span>';
    document.getElementById('months').textContent = remaining + 'か月分';
    document.getElementById('perMonth').textContent = perMonth.toLocaleString() + '円';

    if (isPrepaid) {
      prepaidBreakdown.style.display = 'block';
      document.getElementById('baseRefund').textContent = refund.toLocaleString() + '円';
      document.getElementById('prepaidRefund').textContent = ANNUAL_FEE.toLocaleString() + '円';
    } else {
      prepaidBreakdown.style.display = 'none';
    }

    if (remaining === 0 && !isPrepaid) {
      document.getElementById('caption').textContent = '9月末時点でちょうど契約1年分を使い切っている計算です';
    } else if (isPrepaid) {
      document.getElementById('caption').textContent = '来年度分は1年まるごと未使用のため全額が返金対象です';
    } else {
      const endLabel = m < 10 ? `翌年${m}月` : `${m}月`;
      document.getElementById('caption').textContent = `10月〜${endLabel}分の${remaining}か月が未経過分です`;
    }
  }

  prepaidCheckbox.addEventListener('change', render);

  monthSelect.addEventListener('change', render);
  render();
</script>
</body>
</html>

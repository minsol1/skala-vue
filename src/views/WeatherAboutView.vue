<template>
  <div class="about-page">
    <h3>ℹ️ 서비스 소개</h3>
    <hr />

    <div class="description-box">
      <p>본 앱은 <strong>Vue 3</strong> 및 <strong>Vue Router 4</strong> 기반 제작된 실습용 기상 관측 대시보드 시스템입니다.</p>
      <ul>
        <li><code>components/exercise/</code> 폴더 내부의 독립 부품 연동</li>
        <li>클라이언트 사이드 라우팅을 통한 새로고침 없는 화면 전환</li>
        <li>URL 쿼리 스트링 매핑을 활용한 실시간 검색 상태 동기화</li>
      </ul>
    </div>

    <RouterLink to="/" class="home-btn">대시보드 홈으로 이동</RouterLink>
  </div>
</template>

<style scoped>
.status-bar {
  margin: 20px 0 0;
  padding: 12px;
  border-radius: 10px;
  background: #31c172;
  color: #f9fafb;
  text-align: center;
  font-size: 0.9rem;
}

.home-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
  padding: 12px 18px;
  border-radius: 10px;
  background: #31c172;
  color: #f9fafb;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  max-width: 760px;
}

.about-page {
  max-width: 760px;
  margin: 24px auto;
  padding: 28px;
  border-radius: 20px;
  background: linear-gradient(180deg, #ffffff 0%, #f8fbff 100%);
  box-shadow: 0 14px 36px rgba(15, 23, 42, 0.08);
  color: #1f2937;
}

.about-page h3 {
  margin: 0;
  font-size: 1.35rem;
  color: #0f172a;
}

.about-page hr {
  margin: 14px 0 20px;
  border: 0;
  height: 1px;
  background: linear-gradient(90deg, #dbeafe 0%, #cbd5e1 50%, #dbeafe 100%);
}

.description-box {
  padding: 20px;
  border: 1px solid #e2e8f0;
  border-radius: 16px;
  background: #ffffff;
}

.description-box p {
  margin: 0 0 14px;
  line-height: 1.7;
  color: #334155;
}

.description-box ul {
  margin: 0;
  padding-left: 20px;
  color: #475569;
  line-height: 1.8;
}

.description-box li+li {
  margin-top: 6px;
}



</style>

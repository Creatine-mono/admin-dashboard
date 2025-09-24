<template>
  <div class="dashboard-page">
    <h1>DLP 대시보드</h1>

    <div class="summary-cards">
      <div class="card">
        <h4>총 차단 횟수 (이번 달)</h4>
        <p class="stat-number">1,234회</p>
      </div>
      <div class="card">
        <h4>주요 차단 IP</h4>
        <p class="stat-number">192.168.1.10</p>
        <p class="stat-detail">(15회)</p>
      </div>
      <div class="card">
        <h4>가장 많이 유출된 라벨</h4>
        <p class="stat-number">전화번호</p>
        <p class="stat-detail">(45회)</p>
      </div>
      <div class="card">
        <h4>오늘 탐지된 위협</h4>
        <p class="stat-number threat">23건</p>
      </div>
    </div>

    <div class="chart-grid">
      <div class="chart-container">
        <h3>IP별 요청 대비 차단 횟수</h3>
        <IpAccessChart :chartData="ipAccessData" />
      </div>

      <div class="chart-container">
        <h3>라벨별 분기 유출 횟수</h3>
        <QuarterlyLeakChart :chartData="quarterlyLeakData" />
      </div>

      <div class="chart-container">
        <h3>시간대별 활동</h3>
        <div class="chart-placeholder">
          시간대별 활동 차트
        </div>
      </div>

      <div class="recent-alerts">
        <h3>최근 알림</h3>
        <ul class="alert-list">
          <li class="alert-high">
            <span class="alert-time">14:30</span>
            <span class="alert-msg">주민등록번호 유출 시도 차단 (IP: 192.168.1.15)</span>
          </li>
          <li class="alert-medium">
            <span class="alert-time">14:25</span>
            <span class="alert-msg">신용카드 번호 탐지 (사용자: admin)</span>
          </li>
          <li class="alert-low">
            <span class="alert-time">14:20</span>
            <span class="alert-msg">이메일 주소 필터링 적용</span>
          </li>
          <li class="alert-medium">
            <span class="alert-time">14:15</span>
            <span class="alert-msg">전화번호 대체어 변환 실행</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import IpAccessChart from '../components/IpAccessChart.vue';
import QuarterlyLeakChart from '../components/QuarterlyLeakChart.vue';

export default {
  name: 'DashboardPage',
  components: {
    IpAccessChart,
    QuarterlyLeakChart
  },
  data() {
    return {
      ipAccessData: [
        { ip: '192.168.1.10', requests: 150, blocked: 15, ratio: 10 },
        { ip: '192.168.1.15', requests: 120, blocked: 8, ratio: 6.7 },
        { ip: '192.168.1.20', requests: 90, blocked: 12, ratio: 13.3 },
        { ip: '192.168.1.25', requests: 75, blocked: 5, ratio: 6.7 },
        { ip: '192.168.1.30', requests: 60, blocked: 3, ratio: 5 }
      ],
      quarterlyLeakData: [
        { label: '전화번호', q1: 45, q2: 38, q3: 52, q4: 41 },
        { label: '주민등록번호', q1: 23, q2: 18, q3: 29, q4: 25 },
        { label: '신용카드번호', q1: 15, q2: 12, q3: 18, q4: 14 },
        { label: '이메일주소', q1: 32, q2: 28, q3: 35, q4: 30 },
        { label: '계좌번호', q1: 8, q2: 6, q3: 11, q4: 9 }
      ]
    };
  }
};
</script>

<style scoped>
.dashboard-page {
  padding: 20px;
}

h1 {
  color: #333;
  margin-bottom: 30px;
}

.summary-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 40px;
}

.card {
  background: white;
  padding: 25px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.card h4 {
  margin: 0 0 15px 0;
  color: #666;
  font-size: 14px;
  font-weight: normal;
}

.stat-number {
  font-size: 24px;
  font-weight: bold;
  color: #007bff;
  margin: 0;
}

.stat-number.threat {
  color: #dc3545;
}

.stat-detail {
  font-size: 14px;
  color: #888;
  margin: 5px 0 0 0;
}

.chart-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 30px;
}

.chart-container, .recent-alerts {
  background: white;
  padding: 25px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.chart-container h3, .recent-alerts h3 {
  margin: 0 0 20px 0;
  color: #333;
}

.chart-placeholder {
  height: 200px;
  background: #f8f9fa;
  border: 2px dashed #dee2e6;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #6c757d;
  border-radius: 4px;
}

.alert-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.alert-list li {
  padding: 12px;
  margin-bottom: 8px;
  border-radius: 4px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.alert-high {
  background-color: #ffebee;
  border-left: 4px solid #f44336;
}

.alert-medium {
  background-color: #fff3e0;
  border-left: 4px solid #ff9800;
}

.alert-low {
  background-color: #e8f5e8;
  border-left: 4px solid #4caf50;
}

.alert-time {
  font-weight: bold;
  font-family: monospace;
  color: #666;
  min-width: 50px;
}

.alert-msg {
  color: #333;
  font-size: 14px;
}
</style>
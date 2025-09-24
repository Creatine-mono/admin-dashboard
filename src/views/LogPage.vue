<template>
  <div class="log-page">
    <h1>전체 로그</h1>

    <div class="log-controls">
      <div class="filter-group">
        <label for="ip-filter">IP 필터:</label>
        <input type="text" id="ip-filter" v-model="filterIp" placeholder="IP 주소">
      </div>

      <div class="filter-group">
        <label for="user-filter">사용자 필터:</label>
        <input type="text" id="user-filter" v-model="filterUser" placeholder="사용자 ID">
      </div>

      <div class="filter-group">
        <label for="date-filter">날짜:</label>
        <input type="date" id="date-filter" v-model="selectedDate">
      </div>

      <button @click="refreshLogs" class="refresh-button">새로고침</button>
    </div>

    <div class="log-table-container">
      <table>
        <thead>
          <tr>
            <th>PC IP</th>
            <th>아이디</th>
            <th>시간</th>
            <th>입력 문장</th>
            <th>판정</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="log in filteredLogs" :key="log.id">
            <td>{{ log.ip }}</td>
            <td>{{ log.userId }}</td>
            <td>{{ new Date(log.timestamp).toLocaleString('ko-KR') }}</td>
            <td class="log-sentence" :title="log.sentence">{{ log.sentence }}</td>
            <td>
              <button class="btn btn-true" @click="markAsValid(log.id)">정탐</button>
              <button class="btn btn-false" @click="markAsInvalid(log.id)">오탐</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LogPage',
  data() {
    return {
      filterIp: '',
      filterUser: '',
      selectedDate: '',
      logs: [
        {
          id: 1,
          ip: '192.168.1.10',
          userId: 'user_a',
          timestamp: '2025-09-25T10:00:00Z',
          sentence: '제 전화번호는 010-1234-5678 입니다.'
        },
        {
          id: 2,
          ip: '10.0.0.5',
          userId: 'user_b',
          timestamp: '2025-09-25T10:05:12Z',
          sentence: '이메일 주소는 test@example.com 입니다.'
        },
        {
          id: 3,
          ip: '192.168.1.12',
          userId: 'user_c',
          timestamp: '2025-09-25T11:20:30Z',
          sentence: '회의록에 나온 주민번호는 900101-1234567 입니다.'
        },
        {
          id: 4,
          ip: '192.168.1.15',
          userId: 'admin',
          timestamp: '2025-09-25T12:30:45Z',
          sentence: '계좌번호 123-456-789012 로 입금 부탁드립니다.'
        },
        {
          id: 5,
          ip: '10.0.0.8',
          userId: 'user_d',
          timestamp: '2025-09-25T13:15:20Z',
          sentence: '신용카드 번호는 1234-5678-9012-3456 입니다.'
        },
        {
          id: 6,
          ip: '192.168.1.20',
          userId: 'user_e',
          timestamp: '2025-09-25T14:45:10Z',
          sentence: '제 개인정보: 홍길동, 010-9999-8888, hong@email.com'
        },
        {
          id: 7,
          ip: '172.16.0.1',
          userId: 'manager',
          timestamp: '2025-09-25T15:20:33Z',
          sentence: '보안팀 연락처는 02-1234-5678 입니다.'
        }
      ]
    };
  },
  computed: {
    filteredLogs() {
      return this.logs.filter(log => {
        const ipMatch = !this.filterIp || log.ip.includes(this.filterIp);
        const userMatch = !this.filterUser || log.userId.toLowerCase().includes(this.filterUser.toLowerCase());
        const dateMatch = !this.selectedDate || log.timestamp.startsWith(this.selectedDate);
        return ipMatch && userMatch && dateMatch;
      });
    }
  },
  methods: {
    refreshLogs() {
      console.log('로그 새로고침');
      // 실제로는 API에서 로그 데이터 재조회
    },
    markAsValid(logId) {
      console.log(`로그 ${logId} - 정탐으로 표시`);
      // 실제로는 API로 정탐 처리
    },
    markAsInvalid(logId) {
      console.log(`로그 ${logId} - 오탐으로 표시`);
      // 실제로는 API로 오탐 처리
    }
  }
};
</script>

<style scoped>
.log-page {
  padding: 20px;
}

h1 {
  color: #333;
  margin-bottom: 30px;
}

.log-controls {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
  align-items: end;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.filter-group label {
  font-weight: bold;
  color: #555;
  font-size: 14px;
}

.filter-group input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  width: 150px;
}

.refresh-button {
  padding: 8px 16px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
}

.refresh-button:hover {
  background-color: #218838;
}

.log-table-container {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  min-width: 800px;
}

th, td {
  padding: 12px 15px;
  border-bottom: 1px solid #ddd;
  text-align: left;
}

th {
  background-color: #f8f9fa;
  font-weight: bold;
  color: #555;
  position: sticky;
  top: 0;
}

tr:hover {
  background-color: #f8f9fa;
}

.log-sentence {
  max-width: 400px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  cursor: help;
}

.btn {
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
  font-weight: bold;
  margin-right: 5px;
  color: white;
  transition: background-color 0.3s;
}

.btn-true {
  background-color: #28a745;
}

.btn-true:hover {
  background-color: #218838;
}

.btn-false {
  background-color: #dc3545;
}

.btn-false:hover {
  background-color: #c82333;
}

tbody tr:nth-child(even) {
  background-color: #f9f9f9;
}

tbody tr:nth-child(even):hover {
  background-color: #f1f1f1;
}
</style>
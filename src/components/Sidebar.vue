<template>
  <aside class="sidebar">
    <div class="sidebar-header">
      <h3>DLP Admin</h3>
    </div>
    <nav class="sidebar-nav">
      <router-link to="/dashboard">대시보드</router-link>
      <router-link to="/logs">전체 로그</router-link>
    </nav>
    <div class="sidebar-controls">
      <h4>기능 제어</h4>
      <div class="control-group">
        <label>라벨 필터링</label>
        <div class="toggle-switch">
          <input type="checkbox" id="label-toggle" v-model="controls.labelFilter">
          <label for="label-toggle"></label>
        </div>
      </div>
       <div class="control-group">
        <label>AI 모델 활성화</label>
        <div class="toggle-switch">
          <input type="checkbox" id="model-toggle" v-model="controls.modelActive">
          <label for="model-toggle"></label>
        </div>
      </div>
      <div class="control-group">
        <label>정책: <strong>차단</strong></label>
        <div class="toggle-switch">
          <input type="checkbox" id="block-toggle" v-model="controls.policyBlock">
          <label for="block-toggle"></label>
        </div>
      </div>
      <div class="control-group">
        <label>정책: <strong>대체어 변환</strong></label>
        <div class="toggle-switch">
          <input type="checkbox" id="replace-toggle" v-model="controls.policyReplace">
          <label for="replace-toggle"></label>
        </div>
      </div>
    </div>
    <div class="sidebar-mcp">
      <h4>AI 제어 (자연어)</h4>
      <input type="text" placeholder="예: '전화번호 라벨 끄고 차단 기능 활성화'" v-model="mcpQuery" @keyup.enter="handleMcpQuery">
      <button @click="handleMcpQuery">실행</button>
    </div>
  </aside>
</template>

<script>
export default {
  name: 'Sidebar',
  data() {
    return {
      controls: {
        labelFilter: true,
        modelActive: true,
        policyBlock: true,
        policyReplace: false,
      },
      mcpQuery: '',
    };
  },
  methods: {
    handleMcpQuery() {
      alert(`자연어 쿼리 실행 (백엔드 전송): "${this.mcpQuery}"`);
      this.mcpQuery = '';
    }
  }
};
</script>

<style scoped>
.sidebar {
  width: 250px;
  background-color: #2c3e50;
  color: white;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  display: flex;
  flex-direction: column;
  padding: 20px;
  box-sizing: border-box;
}

.sidebar-header h3 {
  margin: 0;
  text-align: center;
  font-size: 24px;
}

.sidebar-nav {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
}

.sidebar-nav a {
  color: white;
  text-decoration: none;
  padding: 15px 10px;
  border-radius: 4px;
  margin-bottom: 10px;
  transition: background-color 0.3s;
}

.sidebar-nav a.router-link-exact-active, .sidebar-nav a:hover {
  background-color: #34495e;
}

.sidebar-controls {
  margin-top: 40px;
  border-top: 1px solid #444;
  padding-top: 20px;
}

.sidebar-controls h4 {
  margin: 0 0 20px 0;
  font-size: 16px;
  color: #ecf0f1;
}

.control-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.control-group label {
  font-size: 14px;
  color: #bdc3c7;
}

.toggle-switch {
  position: relative;
  width: 50px;
  height: 24px;
}

.toggle-switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.toggle-switch label {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #7f8c8d;
  transition: 0.4s;
  border-radius: 24px;
}

.toggle-switch label:before {
  position: absolute;
  content: "";
  height: 18px;
  width: 18px;
  left: 3px;
  bottom: 3px;
  background-color: white;
  transition: 0.4s;
  border-radius: 50%;
}

.toggle-switch input:checked + label {
  background-color: #27ae60;
}

.toggle-switch input:checked + label:before {
  transform: translateX(26px);
}

.sidebar-mcp {
  margin-top: auto;
  border-top: 1px solid #444;
  padding-top: 20px;
}

.sidebar-mcp h4 {
  margin: 0 0 15px 0;
  font-size: 16px;
  color: #ecf0f1;
}

.sidebar-mcp input {
  width: 100%;
  padding: 8px;
  border-radius: 4px;
  border: none;
  margin-bottom: 10px;
  box-sizing: border-box;
  font-size: 12px;
}

.sidebar-mcp button {
  width: 100%;
  padding: 8px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
  transition: background-color 0.3s;
}

.sidebar-mcp button:hover {
  background-color: #0056b3;
}
</style>
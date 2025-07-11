<template>
  <div class="instances-tab" :data-theme="currentTheme" :class="themeClasses">
    <!-- 加载状态指示器，减少空白屏幕时间 -->
    <div v-if="isLoading" class="loading-container">
      <div class="loading loading-spinner loading-lg text-primary"></div>
      <p class="mt-4 text-base-content/70">正在加载实例管理页面...</p>
    </div> <!-- 内容容器 - 当showInstanceDetail为true时显示实例详情，为false时显示实例列表 -->
    <div v-else>
      <InstanceDetailView v-if="showInstanceDetail" :instance="currentInstance" @back="closeInstanceDetail" />

      <!-- 实例列表组件 - 当showInstanceDetail为false时显示 -->
      <InstancesList v-else :instances="instancesData" @refresh-instances="loadInstances"
        @toggle-instance="handleToggleInstance" @view-instance="openInstanceDetail" />
    </div>

    <!-- Bot配置抽屉 -->
    <BotConfigDrawer 
      :is-open="showBotConfigDrawer"
      :instance-id="botConfigInstanceId"
      :instance-name="botConfigInstanceName"
      @close="closeBotConfigDrawer"
    />
  </div>
</template>

<script setup>
import { ref, inject, onMounted, onBeforeUnmount, computed, provide } from 'vue';
import InstancesList from './instances/InstancesList.vue';
import InstanceDetailView from './instances/InstanceDetailView.vue';
import BotConfigDrawer from './instances/BotConfigDrawer.vue';
import toastService from '@/services/toastService';

// 引入API服务
import { instancesApi } from '@/services/api';

// 注入全局事件总线
const emitter = inject('emitter', null);

// 实例状态
const instancesData = ref([]);
const showInstanceDetail = ref(false);
const isLoading = ref(true); // 添加加载状态
const currentInstance = ref({
  name: '',
  path: '',
  status: 'stopped'
});
const initialSettingsTab = ref('basic'); // 新增：用于记录实例设置的初始标签页

// Bot配置抽屉状态
const showBotConfigDrawer = ref(false);
const botConfigInstanceId = ref('');
const botConfigInstanceName = ref('');

// 提供showInstanceDetail状态给子组件
provide('showInstanceDetail', showInstanceDetail);

// 计算属性获取当前主题
const currentTheme = computed(() => {
  return inject('currentTheme', ref('light'));
});

// 是否为暗色模式
const isDarkMode = computed(() => {
  return inject('darkMode', ref(false));
});

// 主题类计算
const themeClasses = computed(() => {
  return {
    'dark-mode': isDarkMode.value,
    'theme-dark': currentTheme.value === 'dark',
    'theme-light': currentTheme.value === 'light' || !currentTheme.value
  };
});

// 加载实例列表方法
const loadInstances = async () => {
  try {
    isLoading.value = true; // 开始加载

    // 引入实例API适配器
    const { adaptInstancesList } = await import('@/utils/apiAdapters');

    // 使用API获取实例列表，通过适配器处理数据
    const response = await instancesApi.getInstances();
    instancesData.value = adaptInstancesList(response);
    console.log('获取到' + instancesData.value.length + '个实例');
  } catch (error) {
    console.error('获取实例列表失败:', error);
    // 清空实例列表，显示空状态
    instancesData.value = [];
    toastService.error('获取实例列表失败: ' + (error.message || '未知错误'));
  } finally {
    // 确保最小加载时间，避免闪烁
    setTimeout(() => {
      isLoading.value = false;
    }, 200);
  }
};

// 打开实例详情
const openInstanceDetail = (instance) => {
  // 直接切换
  currentInstance.value = instance;
  showInstanceDetail.value = true;
};

// 关闭实例详情
const closeInstanceDetail = () => {
  showInstanceDetail.value = false;
  // 保持WebSocket连接活跃，不清理连接
  console.log('关闭实例详情页面，保持WebSocket连接');
};

// 打开Bot配置抽屉
const openBotConfigDrawer = (instance) => {
  botConfigInstanceId.value = instance.id;
  botConfigInstanceName.value = instance.name;
  showBotConfigDrawer.value = true;
  console.log('打开Bot配置抽屉:', instance);
};

// 关闭Bot配置抽屉
const closeBotConfigDrawer = () => {
  showBotConfigDrawer.value = false;
  botConfigInstanceId.value = '';
  botConfigInstanceName.value = '';
  console.log('关闭Bot配置抽屉');
};

// 处理实例启停
const handleToggleInstance = async (instance) => {
  const index = instancesData.value.findIndex(item => item.id === instance.id || item.name === instance.name);
  if (index === -1) return;

  const newStatus = instance.status === 'running' ? 'stopped' : 'running';
  instancesData.value[index].status = newStatus === 'running' ? 'starting' : 'stopping';

  try {
    // 调用真实的API来启停实例
    if (newStatus === 'running') {
      await instancesApi.startInstance(instance.id || instance.name);
    } else {
      await instancesApi.stopInstance(instance.id || instance.name);
    }

    // 更新状态为最终状态
    instancesData.value[index].status = newStatus;
    toastService.success(`实例 ${instance.name} 已${newStatus === 'running' ? '启动' : '停止'}`);
  } catch (error) {
    console.error('实例启停操作失败:', error);
    // 恢复原来的状态
    instancesData.value[index].status = instance.status;
    toastService.error(`实例${newStatus === 'running' ? '启动' : '停止'}失败: ${error.message || '未知错误'}`);
  }
};

// 组件挂载时设置事件监听
onMounted(() => {
  // 初次加载实例列表
  loadInstances();

  // 当前主题
  const currentTheme = document.documentElement.getAttribute('data-theme') || 'light';
  const isDark = currentTheme === 'dark';

  // 立即应用当前主题
  const instancesPanel = document.querySelector('.instances-tab');
  if (instancesPanel) {
    instancesPanel.setAttribute('data-theme', currentTheme);
    if (isDark) {
      instancesPanel.classList.add('dark-mode', 'theme-dark');
      instancesPanel.classList.remove('theme-light');
    } else {
      instancesPanel.classList.remove('dark-mode', 'theme-dark');
      instancesPanel.classList.add('theme-light');
    }
  }

  // 添加主题变更事件监听
  const handleThemeChanged = (event) => {
    const newTheme = event.detail?.theme || localStorage.getItem('theme') || 'light';
    const isDark = newTheme === 'dark';

    // 获取组件根元素
    const instancesPanelElement = document.querySelector('.instances-tab');
    if (instancesPanelElement) {
      // 设置数据主题
      instancesPanelElement.setAttribute('data-theme', newTheme);

      // 添加或移除暗色模式类
      if (isDark) {
        instancesPanelElement.classList.add('dark-mode', 'theme-dark');
        instancesPanelElement.classList.remove('theme-light');
      } else {
        instancesPanelElement.classList.remove('dark-mode', 'theme-dark');
        instancesPanelElement.classList.add('theme-light');
      }
    }
  };

  window.addEventListener('theme-changed', handleThemeChanged);

  // 监听来自其他页面的实例详情查看请求
  if (emitter) {
    emitter.on('view-instance-details', (instance) => {
      console.log('接收到查看实例详情请求:', instance);
      
      // 先确保实例列表已加载
      if (instancesData.value.length === 0) {
        // 如果实例列表为空，先加载实例列表
        loadInstances().then(() => {
          // 查找对应的实例
          const targetInstance = instancesData.value.find(inst => 
            inst.id === instance.id || inst.name === instance.name
          );
          
          if (targetInstance) {
            openInstanceDetail(targetInstance);
          } else {
            console.warn('未找到指定的实例:', instance);
            toastService.warning(`未找到实例: ${instance.name || instance.id}`);
          }
        });
      } else {
        // 查找对应的实例
        const targetInstance = instancesData.value.find(inst => 
          inst.id === instance.id || inst.name === instance.name
        );
        
        if (targetInstance) {
          openInstanceDetail(targetInstance);
        } else {
          console.warn('未找到指定的实例:', instance);
          toastService.warning(`未找到实例: ${instance.name || instance.id}`);
        }
      }
    });

    // 监听打开Bot配置的请求
    emitter.on('open-bot-config', (instance) => {
      console.log('接收到打开Bot配置请求:', instance);
      openBotConfigDrawer(instance);
    });
  }
  // 组件卸载时移除事件监听
  onBeforeUnmount(() => {
    window.removeEventListener('theme-changed', handleThemeChanged);
    window.removeEventListener('theme-changed-after', handleThemeChanged);
    
    // 移除事件监听
    if (emitter) {
      emitter.off('view-instance-details');
      emitter.off('open-bot-config');
    }
  });
});
</script>

<style scoped>
.instances-tab {
  display: flex;
  flex-direction: column;
  height: 100vh;
  min-height: 100vh;
  width: 100%;
  overflow: hidden; /* 保持隐藏，让子容器处理滚动 */
  position: relative;
  margin: 0;
  padding: 0 0 1.5rem 0;
  box-sizing: border-box;
  /* 与外部CSS保持一致的背景设置 */
  background-image: linear-gradient(135deg,
      hsl(var(--p) / 0.2) 0%,
      hsl(var(--p) / 0.5) 100%);
  background-attachment: local;
  background-size: cover;
  left: 0;
}

/* 加载状态样式 */
.loading-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  min-height: 50vh;
}
</style>

<template>
  <Header />
  <NavHeader />

  <div class="main-content">
    <div
      v-if="
        RewardDisciplineStore.error &&
        RewardDisciplineStore.error.includes('🔒')
      "
      class="auth-error-box"
    >
      <p>{{ RewardDisciplineStore.error }}</p>
    </div>

    <!-- Khen thưởng -->
    <section class="section">
      <h2 class="section-title">🎖️ KHEN THƯỞNG</h2>
      <div class="card-list">
        <div class="card" v-for="(reward, index) in rewards" :key="index">
          <h3>Đoàn viên: {{ reward.recipientName }}</h3>
          <p>Mã SV: {{ reward.recipientMaSV }}</p>
          <p>Lý do: {{ reward.description }}</p>
          <p>Ngày duyệt: {{ reward.createDate }}</p>
          <p>Đề xuất bởi: {{ reward.proposerName }}</p>
          <p v-if="reward.rejectReason">
            Lý do từ chối: {{ reward.rejectReason }}
          </p>
        </div>
      </div>
    </section>

    <!-- Kỷ luật -->
    <section class="section">
      <h2 class="section-title">⚠️ KỶ LUẬT</h2>
      <div class="card-list">
        <div
          class="card warning"
          v-for="(penalty, index) in penalties"
          :key="index"
        >
          <h3>Đoàn viên: {{ penalty.recipientName }}</h3>
          <p>Lý do: {{ penalty.description }}</p>
          <p>Ngày duyệt: {{ penalty.createDate }}</p>
          <p>Đề xuất bởi: {{ penalty.proposerName }}</p>
          <p v-if="penalty.rejectReason">
            Lý do từ chối: {{ penalty.rejectReason }}
          </p>
        </div>
      </div>
    </section>
  </div>

  <Footer />
</template>

<script setup>
import Header from "../components/Header.vue";
import NavHeader from "../components/NavHeader.vue";
import Footer from "../components/Footer.vue";
import { onMounted, ref } from "vue";
import { useRewardDisciplineStore } from "../store/RewardDisciplineStore";

const RewardDisciplineStore = useRewardDisciplineStore();
const rewards = ref([]);
const penalties = ref([]);

onMounted(async () => {
  await RewardDisciplineStore.GetListReward();
  await RewardDisciplineStore.GetListDiscipline();

  rewards.value = RewardDisciplineStore.listReward.map((item) => ({
    ...item,
    createDate: item.createDate.split("T")[0],
  }));

  penalties.value = RewardDisciplineStore.listDiscipline.map((item) => ({
    ...item,
    createDate: item.createDate.split("T")[0],
  }));
});
</script>

<style scoped>
.main-content {
  padding: 20px;
  background-color: #f9f9f9;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

.section {
  margin-bottom: 40px;
}

.section-title {
  font-size: 26px;
  font-weight: bold;
  margin-bottom: 20px;
  border-left: 6px solid #1890ff;
  padding-left: 12px;
  color: #222;
}

.card-list {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}

.card {
  background-color: #e8f5e9; /* xanh nhạt cho khen thưởng */
  padding: 16px;
  border-radius: 12px;
  border-left: 4px solid #28a745; /* viền xanh cho khen thưởng */
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  flex: 1 1 calc(33.333% - 16px);
  min-width: 250px;
  transition: transform 0.3s, box-shadow 0.3s;
}

.card.warning {
  background-color: #fff1f0; /* đỏ nhạt cho kỷ luật */
  border-left: 4px solid #ff4d4f;
}

.card:hover {
  transform: translateY(-5px);
}

.card:not(.warning):hover {
  box-shadow: 0 8px 16px rgba(40, 167, 69, 0.3); /* hiệu ứng hover khen thưởng */
}

.card.warning:hover {
  box-shadow: 0 8px 16px rgba(255, 77, 79, 0.3); /* hiệu ứng hover kỷ luật */
}

.card h3 {
  margin: 0 0 8px;
  font-size: 18px;
  color: #007bff;
}

.card p {
  margin: 4px 0;
  color: #555;
}

.card p:last-child {
  margin-bottom: 0;
}

/* Responsive cho mobile */
@media (max-width: 768px) {
  .card {
    flex: 1 1 100%;
  }
}

.auth-error-box {
  text-align: center;
  padding: 20px;
  background-color: #fff3cd;
  border-radius: 10px;
  color: #856404;
  font-weight: bold;
  margin-bottom: 20px;
}
</style>

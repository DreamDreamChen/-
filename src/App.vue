<template>
  <div class="home">
    <div class="columns">
      <div class="column" v-for="item in items" :key="item.id">
        <div class="card">
          <img :src="item.image" alt="Image" />
          <div class="title">{{ item.title }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "Home",
  setup() {
    const items = ref([]); // 图文数据
    const page = ref(1); // 当前页码

    // 生成随机尺寸和颜色的图片
    const generateImage = () => {
      const width = Math.floor(Math.random() * (800 - 200) + 200);
      const height = Math.floor(Math.random() * (800 - 500) + 500);
      const color = Math.floor(Math.random() * 16777215).toString(16);
      return `https://via.placeholder.com/${width}x${height}/${color}`;
    };

    // 生成随机标题
    const generateTitle = () => {
      const titles = [
        "今天的你可以主动赞美他人，表现得友善一些",
        "今天的你会比较感性，适合去做点浪漫、艺术的事",
        "你要合理发挥行动力，不要把精力浪费在毫无意义的争辩与发泄情绪上",
        "你可以推翻现存的任何事，只要你有想法、有创意",
        "今天的你可以真诚地许下一个愿望，主动感受生活的美好",
        "要合理发挥行动力，不要把精力浪费在毫无意义的争辩与发泄情绪上",
        "生活的意义正在于不断探索未知、不断丰富自己。",
        "拖延是一段长长的痛苦，但如果你勇敢开始，或许就只需要痛苦那一下。",
        "把有限的精力投入到眼下最值得关注的事情上，才有可能充分发挥自己的潜能，在一个领域中达到优秀。",
        "在繁忙的生活琐事中，不妨留给自己一些独处的时间，把外界的喧嚣关在门外，感受世界的美好、丰盈自己的心境。"
      ];
      const index = Math.floor(Math.random() * titles.length);
      return titles[index];
    };

    // 生产模拟数据
    const generateData = () => {
      const newData = [];
      for (let i = 0; i < 10; i++) {
        const item = {
          id: items.value.length + i,
          image: generateImage(),
          title: generateTitle()
        };
        newData.push(item);
      }
      items.value = items.value.concat(newData);
    };

    // 加载更多数据
    const loadMore = () => {
      page.value++;
      generateData();
      // Toast.success('加载成功');
    };

    // 监听滚动事件，判断是否需要加载更多数据
    const handleScroll = () => {
      const windowHeight = window.innerHeight;
      const scrollTop =
        document.documentElement.scrollTop || document.body.scrollTop;
      const documentHeight =
        document.documentElement.scrollHeight || document.body.scrollHeight;
      if (windowHeight + scrollTop >= documentHeight - 100) {
        loadMore();
      }
    };

    onMounted(() => {
      generateData();
      window.addEventListener("scroll", handleScroll);
    });

    return {
      items,
      loadMore
    };
  }
};
</script>

<style>
.home {
  /* padding: 16px; */
  display: flex;
  flex-direction: column;
}

.columns {
  flex: 1;
  columns: 2;
}

.column {
  /* flex-basis: calc(50% - 10px); */
  break-inside: avoid;
}

.card {
  border-radius: 4px;
  overflow: hidden;
  background-color: #f2f2f2;
  margin-bottom: 16px;
}

.card img {
  width: 100%;
  height: auto;
}

.title {
  padding: 8px;
  font-size: 14px;
  color: #333;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>

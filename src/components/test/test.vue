<template>
  <div class="home">
    <div class="bottom-data">
      <div class="i-card-left" @click="jumpToPro">
        <div class="card-pix">
          <div class="card-icon">
            <img src="@/assets/images/Home/icon_zhuanjia.png" alt="" />
          </div>
          <div class="card-title">专家信息</div>
          <div class="card-to-num"></div>
        </div>
      </div>
      <div class="i-card-right" @click="jumpToMeeting">
        <div class="card-pix">
          <div class="card-icon">
            <img src="@/assets/images/Home/icon_huiyi.png" alt="" />
          </div>
          <div class="card-title">会议信息</div>
          <div class="card-to-num"></div>
        </div>
      </div>
    </div>
    <div class="swiper-wrap">
      <div class="swiper-btn-ic">
        <Button
          @click="prevBtn"
          size="large"
          :type="activeBtn === '0' ? 'primary' : 'default'"
        >
          信息研判流程
        </Button>
        <Button
          @click="nextBtn"
          size="large"
          :type="activeBtn === '1' ? 'primary' : 'default'"
        >
          项目管理流程
        </Button>
      </div>
        <Carousel autoplay :beforeChange="beforeChange" ref="carousel">
          <div style="position: relative; margin-top: 80px; height: 100%">
            <div class="img-wrap">
              <img src="@/assets/images/Home/image_xiangmujindu.png" alt="" />
            </div>
            <div class="sider-bar-list">
              <div class="icon-wrap">
                <div class="avter">
                  <img src="@/assets/images/Home/touxiang1.png" alt="" />
                </div>
                <div class="desc">
                  <div class="desc-a-1">信息管理员</div>
                </div>
              </div>

              <div class="icon-wrap">
                <div class="avter">
                  <img src="@/assets/images/Home/touxiang2.png" alt="" />
                </div>
                <div class="desc">
                  <div class="desc-a-2">信息研判人员</div>
                </div>
              </div>

              <div class="icon-wrap">
                <div class="avter">
                  <img src="@/assets/images/Home/touxiang3.png" alt="" />
                </div>
                <div class="desc">
                  <div class="desc-a-3">研究中心人员</div>
                </div>
              </div>
            </div>
          </div>
          <div style="position: relative; margin-top: 100px">
          <div class="img-wrap">
            <img src="@/assets/images/Home/image_yanjiujieguo.png" alt="" />
          </div>
          <div class="sider-bar-list">
            <div class="icon-wrap">
              <div class="avter">
                <img src="@/assets/images/Home/touxiang1.png" alt="" />
              </div>
              <div class="desc">
                <div class="desc-a-1">项目负责人</div>
                <div class="desc-a-1">会议管理员</div>
                <div class="desc-a-1">办公室人员</div>
              </div>
            </div>
            <div class="icon-wrap">
              <div class="avter">
                <img src="@/assets/images/Home/touxiang2.png" alt="" />
              </div>
              <div class="desc">
                <div class="desc-a-2">任务接收人员</div>
              </div>
            </div>
            <div class="icon-wrap">
              <div class="avter">
                <img src="@/assets/images/Home/touxiang3.png" alt="" />
              </div>
              <div class="desc">
                <div class="desc-a-3">信息管理员</div>
              </div>
            </div>
            <div class="icon-wrap">
              <div class="avter">
                <img src="@/assets/images/Home/touxiang4.png" alt="" />
              </div>
              <div class="desc">
                <div class="desc-a-4">信息初判人员</div>
              </div>
            </div>

            <div class="icon-wrap">
              <div class="avter">
                <img src="@/assets/images/Home/touxiang5.png" alt="" />
              </div>
              <div class="desc">
                <div class="desc-a-5">缺陷召回人员</div>
              </div>
            </div>

            <div class="icon-wrap">
              <div class="avter">
                <img src="@/assets/images/Home/touxiang6.png" alt="" />
              </div>
              <div class="desc">
                <div class="desc-a-6">科发部人员</div>
              </div>
            </div>

            <div class="icon-wrap">
              <div class="avter">
                <img src="@/assets/images/Home/touxiang7.png" alt="" />
              </div>
              <div class="desc">
                <div class="desc-a-7">风险检测人员</div>
              </div>
            </div>
          </div>
        </div>
        </Carousel>
    </div>
  </div>
</template>

<script>
import "./index.css";
import { Carousel, Button } from "ant-design-vue";

export default {
  name: "HomePage",
  components: {
    Carousel,
    Button,
  },
  data() {
    return {
      activeBtn: "0",
      startVal: 0,
      zjslNum: 0,
      zjxxNum: 0,
    };
  },
  mounted() {},
  methods: {
    beforeChange(from, to) {
      this.activeBtn = `${to}`;
    },
    prevBtn() {
      this.$refs.carousel.goTo(0);
    },
    nextBtn() {
      this.$refs.carousel.goTo(1);
    },

    // 跳转至专家信息
    jumpToPro() {
      this.$router.push("/professor/allProfessorInfo");
    },
    // 跳转至会议信息
    jumpToMeeting() {
      this.$router.push("/meeting/allMeetingInfo");
    },
  },
};
</script>

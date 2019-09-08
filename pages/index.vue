<template>
  <div class="container">
    <!-- 轮播图组件:el-carousel -->
    <!-- arrow: 左右切换的箭头总是显示的 -->
    <el-carousel arrow="always">
      <!-- el-carousel-item 是幻灯片的每一项 -->
      <el-carousel-item v-for="(item, index) in banners" :key="index">
        <!-- 轮播图的背景图片
        background-size：控制背景图片的大小，自适应宽高-->
        <div
          class="banner-image"
          :style="`
            background:url(${item}) center  no-repeat;
            background-size: cover;
            `"
        ></div>
      </el-carousel-item>

      <!-- 搜索框 -->
        <div class="banner-content">
            <div class="search-bar">
                
                <!-- tab栏 -->
                <el-row 
                type="flex" 
                class="search-tab">
                    <span 
                    v-for="(item, index) in options" 
                    :key="index" 
                    :class="{active: index === currentOption}"
                    @click="handleOption(index)">
                        <i>{{item.name}}</i>
                    </span>
                </el-row>
                
                <!-- 输入框 -->
                <el-row 
                type="flex" 
                align="middle" 
                class="search-input">
                    <input 
                    :placeholder="options[currentOption].placeholder" 
                    v-model="searchValue"
                    @keyup.enter="handleSearch"/>
                    <i class="el-icon-search" @click="handleSearch"></i>
                </el-row>
            </div>
        </div>

    </el-carousel>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 轮播图数组
      banners: [
        "https://img.chenbie.com/d/file/051022/xqwfbu5ykr4.jpg",
        "https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=3788087385,896499222&fm=173&app=49&f=JPEG?w=640&h=474&s=54168775C940695F441745C00300E0BA",
        "https://img.chenbie.com/d/file/051022/quugu02dr0y.jpg"
      ],
      options: [      // 搜索框tab选项
                {
                    name: "攻略", 
                 	placeholder: "搜索城市", 
                 	pageUrl: "/post?city="
                },
                {
                    name: "酒店", 
                    placeholder: "请输入城市搜索酒店", 
                    pageUrl: "/hotel?city="},
                {
                    name: "机票", 
                    placeholder: "请输入出发地", 
                    pageUrl: "/air"
                }
            ],
            searchValue: "",    // 搜索框的值
            currentOption: 0,   // 当前选中的选项      
    };
  },
  mounted(){
        
    },
  methods: {
        // 切换tab栏时候触发
handleOption(index){
    // 设置当前tab
    this.currentOption = index;

    // 如果切换的机票tab，那么直接跳转到机票首页
    const item = this.options[index];
    if(item.name === "机票"){
        return this.$router.push(item.pageUrl);
    }
},
     // 搜索时候触发
handleSearch(){
    const item = this.options[this.currentOption];
    // 跳转时候给对应的页面url加上搜索内容参数
    this.$router.push(item.pageUrl + this.searchValue);
}
    },
};
</script>


<style scoped lang="less">
.container{
    min-width:1000px;
    margin:0 auto;
    position:relative;

    /deep/ .el-carousel__container{
        height:700px;
    }

    .banner-image{
        width:100%;
        height:100%;
    }

    .banner-content{
        z-index:9;
        width:1000px;
        position:absolute;
        left:50%;
        top:45%;
        margin-left: -500px;
        border-top:1px transparent solid;

        .search-bar{
            width:552px;
            margin:0 auto;
        }

        .search-tab{
            .active{
                i{
                color:#333;
                }
                &::after{
                background: #eee;
                }
            }

            span{
                width:82px;
                height:36px;
                display:block;
                position: relative;
                margin-right:8px;
                cursor: pointer;

                i{
                position:absolute;
                z-index:2;
                display: block;
                width:100%;
                height:100%;
                line-height:30px;
                text-align:center;
                color:#fff;
                }

                &:after{
                position: absolute;
                left:0;
                top:0;
                display:block;
                content: "";
                width:100%;
                height:100%;
                border: 1px rgba(255,255,255,.2) solid;
                border-bottom: none;
                transform: scale(1.1,1.3) perspective(.7em) rotateX(2.2deg);
                transform-origin: bottom left;
                background: rgba(0,0,0,.5);
                border-radius:1px 2px 0 0;
                box-sizing:border-box;
                }
            }
        }

        .search-input{
            width:550px;
            height:46px;
            background:#fff;
            border-radius: 0 4px 4px 4px;
            border: 1px rgba(255,255,255,.2) solid;
            border-top:none;
            box-sizing: unset;

            input{
                flex:1;
                height:20px;
                padding: 13px 15px;
                outline: none;
                border:0;
                font-size:16px;
            }

            .el-icon-search{
                cursor :pointer;
                font-size:22px;
                padding:0 10px;
                font-weight:bold;
            }
        }
    }
}
</style>
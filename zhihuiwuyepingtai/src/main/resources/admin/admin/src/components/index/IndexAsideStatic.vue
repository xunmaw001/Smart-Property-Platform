<template>
  <el-aside class="index-aside" width="100%">
    <div class="index-aside-inner menulist">
      <div v-for="item in menuList" :key="item.roleName" v-if="role==item.roleName" class="menulist-item">
        <div class="menulistImg" v-if="false && 1 == 2">
          <el-image :style='{"padding":"0","boxShadow":"0 0 6px rgba(0,0,0,0)","margin":"0","borderColor":"rgba(0,0,0,0)","borderRadius":"0","borderWidth":"0","width":"100%","borderStyle":"solid","height":"auto"}' v-if="'http://codegen.caihongy.cn/20201021/cc7d45d9c8164b58b18351764eba9be1.jpg'" src="http://codegen.caihongy.cn/20201021/cc7d45d9c8164b58b18351764eba9be1.jpg" fit="cover" />
        </div>
		<el-menu :mode="1 == 1? 'horizontal':'vertical'" :unique-opened="true" class="el-menu-demo" default-active="0">
          <el-menu-item index="0" @click="menuHandler('')"><i v-if="false" class="el-icon-menu el-icon-s-home" />首页</el-menu-item>
          <el-submenu :index="1+''">
            <template slot="title">
				<i v-if="false" class="el-icon-menu el-icon-user-solid" />
				<span>个人中心</span>
            </template>
            <el-menu-item :index="1-1" @click="menuHandler('updatePassword')">修改密码</el-menu-item>
            <el-menu-item :index="1-2" @click="menuHandler('center')">个人信息</el-menu-item>
          </el-submenu>
          <el-submenu v-for=" (menu,index) in item.backMenu" :key="menu.menu" :index="index+2+''">
            <template slot="title">
				<i v-if="false" class="el-icon-menu" :class="icons[index]" />
				<span>{{ menu.menu }}</span>
            </template>
            <el-menu-item v-for=" (child,sort) in menu.child" :key="sort" :index="(index+2)+'-'+sort" @click="menuHandler(child.tableName)">{{ child.menu }}</el-menu-item>
          </el-submenu>
        </el-menu>
      </div>
    </div>
  </el-aside>
</template>

<script>
import menu from '@/utils/menu'
export default {
  data() {
    return {
      menuList: [],
      dynamicMenuRoutes: [],
      role: '',
      icons: [
        'el-icon-s-cooperation',
        'el-icon-s-order',
        'el-icon-s-platform',
        'el-icon-s-fold',
        'el-icon-s-unfold',
        'el-icon-s-operation',
        'el-icon-s-promotion',
        'el-icon-s-release',
        'el-icon-s-ticket',
        'el-icon-s-management',
        'el-icon-s-open',
        'el-icon-s-shop',
        'el-icon-s-marketing',
        'el-icon-s-flag',
        'el-icon-s-comment',
        'el-icon-s-finance',
        'el-icon-s-claim',
        'el-icon-s-custom',
        'el-icon-s-opportunity',
        'el-icon-s-data',
        'el-icon-s-check',
        'el-icon-s-grid',
        'el-icon-menu',
        'el-icon-chat-dot-square',
        'el-icon-message',
        'el-icon-postcard',
        'el-icon-position',
        'el-icon-microphone',
        'el-icon-close-notification',
        'el-icon-bangzhu',
        'el-icon-time',
        'el-icon-odometer',
        'el-icon-crop',
        'el-icon-aim',
        'el-icon-switch-button',
        'el-icon-full-screen',
        'el-icon-copy-document',
        'el-icon-mic',
        'el-icon-stopwatch',
      ],
      menulistStyle: '${template.back.menulist.menulistStyle}',
	  menulistBorderBottom: {},
    }
  },
  mounted() {
    const menus = menu.list()
    if(menus) {
        this.menuList = menus
    } else {
        let params = {
            page: 1,
            limit: 1,
            sort: 'id',
        }
        this.$http({
            url: "menu/list",
            method: "get",
            params: params
        }).then(({
            data
        }) => {
            if (data && data.code === 0) {
                this.menuList = JSON.parse(data.data.list[0].menujson);
                this.$storage.set("menus", this.menuList);
            }
        })
    }
    this.role = this.$storage.get('role')
  },
  created(){
    setTimeout(()=>{
      this.menulistStyleChange()
    },10)
    this.icons.sort(()=>{
      return (0.5-Math.random())
    })
	this.lineBorder()
  },
  methods: {
	lineBorder() {
		
          let style = '${template.back.menulist.menulistStyle}'
          let w = '${template.back.menulist.menulistLineWidth}'
          let s = '${template.back.menulist.menulistLineStyle}'
          let c = '${template.back.menulist.menulistLineColor}'
		
		if(style == 'vertical') {
			this.menulistBorderBottom = {
				borderBottomWidth: w,
				borderBottomStyle: s,
				borderBottomColor: c
			}
		} else {
			this.menulistBorderBottom = {
				borderRightWidth: w,
				borderRightStyle: s,
				borderRightColor: c
			}
		}
	},
    menuHandler(name) {
      let router = this.$router
      name = '/'+name
      router.push(name)
    },
    // 菜单
    setMenulistHoverColor(){
      let that = this
	  return;
      this.$nextTick(()=>{
        document.querySelectorAll('.menulist .el-menu-item').forEach(el=>{
          el.addEventListener("mouseenter", e => {
            e.stopPropagation()
            el.style.backgroundColor = "${template.back.menulist.menulistHoverColor}"
          })
          el.addEventListener("mouseleave", e => {
            e.stopPropagation()
			el.style.background = "none"
          })
          el.addEventListener("focus", e => {
            e.stopPropagation()
            el.style.backgroundColor = "${template.back.menulist.menulistHoverColor}"
          })
        })
        document.querySelectorAll('.menulist .el-submenu__title').forEach(el=>{
          el.addEventListener("mouseenter", e => {
            e.stopPropagation()
            el.style.backgroundColor = "${template.back.menulist.menulistHoverColor}"
          })
          el.addEventListener("mouseleave", e => {
            e.stopPropagation()
			el.style.background = "none"
          })
        })
      })
    },
    setMenulistIconColor() {
      this.$nextTick(()=>{
        document.querySelectorAll('.menulist .el-submenu__title .el-submenu__icon-arrow').forEach(el=>{
          el.style.color = "${template.back.menulist.menulistIconColor}"
        })
      })
    },
    menulistStyleChange() {
      this.setMenulistIconColor()
      this.setMenulistHoverColor()
      this.setMenulistStyleHeightChange()
      let str = "1"
      if(1 == str) {
        this.$nextTick(()=>{
          document.querySelectorAll('.el-container .el-container').forEach(el=>{
            el.style.display = "block"
            el.style.paddingTop = "60px" // header 高度
          })
          document.querySelectorAll('.el-aside').forEach(el=>{
            el.style.width = "100%"
            el.style.height = "68px"
            el.style.paddingTop = '0'
          })
          document.querySelectorAll('.index-aside .index-aside-inner').forEach(el=>{
            el.style.paddingTop = '0'
			el.style.width = "100%"
          })
        })
      }
      if(2 === str) {
        this.$nextTick(()=>{
          document.querySelectorAll('.index-aside .index-aside-inner').forEach(el=>{
            el.style.paddingTop = "60px"
          })
        })
      }
    },
    setMenulistStyleHeightChange() {
		return;
      this.$nextTick(()=>{
        document.querySelectorAll('.menulist-item>.el-menu--horizontal>.el-menu-item').forEach(el=>{
          el.style.height = "${template.back.menulist.menulistHeight}"
          el.style.lineHeight = "${template.back.menulist.menulistHeight}"
        })
        document.querySelectorAll('.menulist-item>.el-menu--horizontal>.el-submenu>.el-submenu__title').forEach(el=>{
          el.style.height = "${template.back.menulist.menulistHeight}"
          el.style.lineHeight = "${template.back.menulist.menulistHeight}"
        })
      })
    },
  }
}
</script>
<style lang="scss" scoped>
	.el-container {
		display: block;
	}
  .index-aside {
    position: relative;
    overflow: hidden;
	display: flex;
	flex-wrap: wrap;

    .menulistImg {
		font-size: 0;
		box-sizing: border-box;

      .el-image {
        margin: 0 auto;
        width: 100px;
        height: 100px;
        border-radius: 100%;
        display: block;
      }
    }
	
		
		.el-menu-demo {
		width: 100%;
		height: 68px !important;
		padding: 0px 0px 0px 0px;
		margin: 0px 0px 0px 0px;
		border-radius: 0;
		border-width: 8px 0px 0px 0px !important;
		border-style: dotted !important;
		border-color: rgba(255, 252, 252, 1) !important;
		background:  linear-gradient(var(--publicMainColor), var(--publicSubColor)) !important;
		box-shadow: 0px 0px 0px 0px rgba(255,255,255, 1);
		box-sizing: border-box;
					
		&>.el-menu-item {
			width: 143px;
			height: auto !important;
			line-height: 56px !important;
			padding: 0px 0px 0px 31px;
			margin: 0px 0px 0px 8px;
			color: rgba(0, 0, 0, 1);
			font-size: 14px;
			border-radius: 0px 0px 50% 50%;
			border-width: 0;
			border-style: dotted;
			border-color: rgba(255, 255, 255, 1) !important;
			background-color: var(--publicMainColor) !important;
			box-shadow: 0px 3px 0px 0px rgba(254, 206, 192, 1);
			box-sizing: initial;
			display: flex;
			align-items: center;
			justify-content: flex-start;
			box-sizing: border-box;
            
            .el-icon-menu {
            	margin: 0 2px 0 0;
            	padding: 0;
            	width: 24px;
            	line-height: 24px;
            	color: rgba(0, 0, 0, 1);
            	font-size: 16px;
            	border-radius: 0;
            	border-width: 0;
            	border-style: solid;
            	border-color: #fff;
            	background-color: rgba(255, 215, 0, 0);
            	box-shadow: 0 0 6px rgba(255,255,255,0);
            }
		}
		
		.el-submenu {
			margin: 0px 0px 0px 8px;
		}
		
		& /deep/ .el-submenu__title {
			width: 123px;
			height: auto !important;
			line-height: 56px !important;
			padding: 0px 0px 0px 31px;
			color: rgba(0, 0, 0, 1);
			font-size: 14px;
			border-radius: 0px 0px 50% 50%;
			border-width: 0;
			border-style: dotted;
			border-color: rgba(255, 255, 255, 1) !important;
			background-color: var(--publicMainColor) !important;
			box-shadow: 0px 3px 0px 0px rgba(254, 206, 192, 1);
			box-sizing: initial;
			display: flex;
			align-items: center;
			justify-content: flex-start;
			box-sizing: border-box;
			
			.el-icon-menu {
				margin: 0 2px 0 0;
				padding: 0;
				width: 24px;
				line-height: 24px;
				color: rgba(0, 0, 0, 1);
				font-size: 16px;
				border-radius: 0;
				border-width: 0;
				border-style: solid;
				border-color: #fff;
				background-color: rgba(255, 215, 0, 0);
				box-shadow: 0 0 6px rgba(255,255,255,0);
			}
			
			.el-submenu__icon-arrow {
				margin: 0 10px 0 0;
				padding: 0;
				width: 12px;
				line-height: 12px;
				color: rgba(0, 0, 0, 1) !important;
				font-size: 12px;
				border-radius: 0;
				border-width: 0;
				border-style: solid;
				border-color: #fff;
				background-color: rgba(255,255,255,0);
				box-shadow: 0 0 6px rgba(255,255,255,0);
				position: absolute;
				top: 50%;
				right: 0;
				transform: translateY(-50%);
				text-align: center;
								display: block;
											}
		}
		
		& /deep/ .el-menu.el-menu--inline {
			width: 140px;
			height: auto;
			padding: 0;
			margin: 6px 0px 0px 0px;
			border-radius: 0;
			border-width: 0;
			border-style: solid;
			border-color: rgba(0,0,0,.3);
			background-color: rgba(255, 215, 0, 0);
			box-shadow: 0 0 0px rgba(0, 0, 0, .3);
			
			.el-menu-item {
				width: 100%;
				height: 50px;
				line-height: 50px;
				padding: 0px 0px 0px 0px !important;
				margin: 5px 0px 0px 6px;
				color: #000 !important;
				font-size: 14px;
				border-radius: 0px 0px 50% 50%;
				border-width: 5px;
				border-style: double;
				border-color: rgba(255, 251, 251, 1);
				background-color: var(--publicMainColor) !important;
				box-shadow: 0 0 0px rgba(30, 144, 255, 0);
				text-align: center;
				min-width: auto;
                
                &.is-active {
                    width: 100%;
                    height: 50px;
                    line-height: 50px;
                    padding: 0px 0px 0px 0px !important;
                    margin: 5px 0px 0px 6px;
                    color: rgba(0, 0, 0, 1) !important;
                    font-size: 14px;
                    border-radius: 0px 0px 50% 50%;
                    border-width: 0;
                    border-style: double;
                    border-color: rgba(220, 86, 101, 1);
                    background-color: rgba(255, 255, 255, 0) !important;
                    box-shadow: 0 0 0px rgba(30, 144, 255, 0);
                    text-align: center;
                }
				 
				&:hover {
					width: 100%;
					height: 50px;
					line-height: 50px;
					padding: 0px 0px 0px 0px !important;
					margin: 5px 0px 0px 6px;
					color: #fff !important;
					font-size: 14px;
					border-radius: 0px 0px 50% 50%;
					border-width: 5px;
					border-style: double;
					border-color: rgba(255, 255, 255, 1);
					background-color: rgba(246, 143, 158, 1) !important;
					box-shadow: 0 0 0px rgba(30, 144, 255, 0);
					text-align: center;
				}
			}
		}
	}
  }
</style>
<style>

.el-menu--horizontal .el-menu--popup {
		width: 140px;
		height: auto;
		padding: 0;
		margin: 6px 0px 0px 0px;
		border-radius: 0;
		border-width: 0;
		border-style: solid;
		border-color: rgba(0,0,0,.3);
		background-color: rgba(255, 215, 0, 0);
		box-shadow: 0 0 0px rgba(0, 0, 0, .3);
		min-width: auto;
		}
.el-menu--horizontal .el-menu--popup .el-menu-item {
			width: 100%;
			height: 50px;
			line-height: 50px;
			padding: 0px 0px 0px 0px;
			margin: 5px 0px 0px 6px;
			color: #000 !important;
			font-size: 14px;
			border-radius: 0px 0px 50% 50%;
			border-width: 5px;
			border-style: double;
			border-color: rgba(255, 251, 251, 1);
			background-color: var(--publicMainColor) !important;
			box-shadow: 0 0 0px rgba(30, 144, 255, 0);
			text-align: center;
			min-width: auto;
			} 
	.el-menu--horizontal .el-menu--popup .el-menu-item:hover {
				width: 100%;
				height: 50px;
				line-height: 50px;
				padding: 0px 0px 0px 0px;
				margin: 5px 0px 0px 6px;
				color: #fff !important;
				font-size: 14px;
				border-radius: 0px 0px 50% 50%;
				border-width: 5px;
				border-style: double;
				border-color: rgba(255, 255, 255, 1);
				background-color: rgba(246, 143, 158, 1) !important;
				box-shadow: 0 0 0px rgba(30, 144, 255, 0);
				text-align: center;
			}
</style>

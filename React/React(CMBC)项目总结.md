# CMBC 项目总结

****

## 主页

> 业务逻辑：点击相应内容进行对应跳转
> 技术：轮播图，选项卡，路由

### 轮播图 （使用 swiper 插件实现）

```
import Swiper from 'swiper/dist/js/swiper.js';

componentDidMount () {

        const mySwiper = new Swiper('.swiper-container', {
          autoplay: true,
          auto:5000,
          loop: true,
          pagination : {
              el: '.slidesjs-pagination-item',//焦点类名
              clickable: true,

          }
        });
  }
//结构demo
   <div className="swiper-container sroll-banner swiper-container-horizontal swiper-container-autoheight">
            <div className="swiper-wrapper"
            >
                <div className="swiper-slide swiper-slide-duplicate swiper-slide-next swiper-slide-duplicate-prev"
                    data-swiper-slide-index="0" ><img
                        src="http://images.cmbchina.com/cmbadv/201906/3d8bdd98-5438-41c5-9397-8bf3a342ae12.jpg"
                        width="100%" height="100%" alt="" keys="0" className="J_ScrollImgs"
                     />

                        </div>
                <div className="swiper-slide swiper-slide-duplicate-active swiper-slide-prev swiper-slide-duplicate-next"
                    data-swiper-slide-index="0" ><img
                        src="http://images.cmbchina.com/cmbadv/201906/3d8bdd98-5438-41c5-9397-8bf3a342ae12.jpg"
                        width="100%" height="100%" alt="" keys="0" className="J_ScrollImgs"
                       />

                        </div>
                <div className="swiper-slide swiper-slide-duplicate swiper-slide-active swiper-slide-duplicate-prev"
                    data-swiper-slide-index="0" ><img
                        src="http://images.cmbchina.com/cmbadv/201906/3d8bdd98-5438-41c5-9397-8bf3a342ae12.jpg"
                        width="100%" height="100%" alt="" keys="0" className="J_ScrollImgs"
                      />

                        </div>
            </div>
            <ul className="slidesjs-pagination swiper-pagination-custom">
                <li className="slidesjs-pagination-item"><a className="active"></a></li>
            </ul>
        </div>
```

### 选项卡

```
   handleTab1(idx){

            this.setState({
                activeIdx:idx,
            })

    };
    <!-- 按钮区 -->
     <div data-v-567ff6b7="" id="J_TabFinance" onClick={this.handleTab2.bind(this,0)} className={this.state.activeIndex===0?'col-3 cmb-tab selected':'col-3 cmb-tab '}>理财产品</div>
     <!-- 内容区 -->
    <div data-v-567ff6b7="" id="J_BodyFund" className={this.state.activeIndex===1?'show':null}>
```

## 实时外汇页

> 业务逻辑：根据接口内容渲染
> 技术：渲染，跨域

* 在 package.json 设置以下语句解决跨域
<mark >`  "proxy": "http://m.cmbchina.com/api/rate/"`

* 请求数据

```
    async getContent(){
        const res = await axios.get('/getfxrate')
    }
```

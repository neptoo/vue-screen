<template>
  <div class="container">
    <div class="map-container">
      <button @click="back" class="close-back">返回</button>
      <div id="mapChart"></div>
    </div>
  </div>
</template>
<script>
import * as echarts from "echarts";
let provinces = [
  "pakistan",
  "shanghai",
  "hebei",
  "shanxi",
  "neimenggu",
  "liaoning",
  "jilin",
  "heilongjiang",
  "jiangsu",
  "zhejiang",
  "anhui",
  "fujian",
  "jiangxi",
  "shandong",
  "henan",
  "hubei",
  "hunan",
  "guangdong",
  "guangxi",
  "hainan",
  "sichuan",
  "guizhou",
  "yunnan",
  "xizang",
  "shanxi1",
  "gansu",
  "qinghai",
  "ningxia",
  "xinjiang",
  "beijing",
  "tianjin",
  "chongqing",
  "xianggang",
  "aomen",
  "taiwan",
];
let provincesText = [
  "巴基斯坦",
  "上海市",
  "河北省",
  "山西省",
  "内蒙古自治区",
  "辽宁省",
  "吉林省",
  "黑龙江省",
  "江苏省",
  "浙江省",
  "安徽省",
  "福建省",
  "江西省",
  "山东省",
  "河南省",
  "湖北省",
  "湖南省",
  "广东省",
  "广西壮族自治区",
  "海南省",
  "四川省",
  "贵州省",
  "云南",
  "西藏自治区",
  "陕西省",
  "甘肃省",
  "青海省",
  "宁夏回族自治区",
  "新疆维吾尔自治区",
  "北京市",
  "天津市",
  "重庆市",
  "香港",
  "澳门",
  "台湾",
];

const nameMap = {
  "Azad Kashmir": "阿扎德克什米尔",
  "Federally Administered Tribal Areas": "联邦直辖部落区",
  "Gilgit-Baltistan": "吉尔吉特－巴尔蒂斯坦",
  "Islamabad Capital Territory": "伊斯兰堡",
  "Khyber Pakhtunkhwa": "开伯尔普赫图赫瓦省",
  Punjab: "旁遮普",
  Sindh: "信德省",
  Balochistan: "俾路支斯坦",
  Afghanistan: "阿富汗",
  Singapore: "新加坡",
  Angola: "安哥拉",
  Albania: "阿尔巴尼亚",
  "United Arab Emirates": "阿联酋",
  Argentina: "阿根廷",
  Armenia: "亚美尼亚",
  "French Southern and Antarctic Lands": "法属南半球和南极领地",
  Australia: "澳大利亚",
  Austria: "奥地利",
  Azerbaijan: "阿塞拜疆",
  Burundi: "布隆迪",
  Belgium: "比利时",
  Benin: "贝宁",
  "Burkina Faso": "布基纳法索",
  Bangladesh: "孟加拉国",
  Bulgaria: "保加利亚",
  "The Bahamas": "巴哈马",
  "Bosnia and Herzegovina": "波斯尼亚和黑塞哥维那",
  Belarus: "白俄罗斯",
  Belize: "伯利兹",
  Bermuda: "百慕大",
  Bolivia: "玻利维亚",
  Brazil: "巴西",
  Brunei: "文莱",
  Bhutan: "不丹",
  Botswana: "博茨瓦纳",
  "Central African Republic": "中非共和国",
  Canada: "加拿大",
  Switzerland: "瑞士",
  Chile: "智利",
  China: "中国",
  "Ivory Coast": "象牙海岸",
  Cameroon: "喀麦隆",
  "Democratic Republic of the Congo": "刚果民主共和国",
  "Republic of the Congo": "刚果共和国",
  Colombia: "哥伦比亚",
  "Costa Rica": "哥斯达黎加",
  Cuba: "古巴",
  "Northern Cyprus": "北塞浦路斯",
  Cyprus: "塞浦路斯",
  "Czech Republic": "捷克共和国",
  Germany: "德国",
  Djibouti: "吉布提",
  Denmark: "丹麦",
  "Dominican Republic": "多明尼加共和国",
  Algeria: "阿尔及利亚",
  Ecuador: "厄瓜多尔",
  Egypt: "埃及",
  Eritrea: "厄立特里亚",
  Spain: "西班牙",
  Estonia: "爱沙尼亚",
  Ethiopia: "埃塞俄比亚",
  Finland: "芬兰",
  Fiji: "斐",
  "Falkland Islands": "福克兰群岛",
  France: "法国",
  Gabon: "加蓬",
  "United Kingdom": "英国",
  Georgia: "格鲁吉亚",
  Ghana: "加纳",
  Guinea: "几内亚",
  Gambia: "冈比亚",
  "Guinea Bissau": "几内亚比绍",
  Greece: "希腊",
  Greenland: "格陵兰",
  Guatemala: "危地马拉",
  "French Guiana": "法属圭亚那",
  Guyana: "圭亚那",
  Honduras: "洪都拉斯",
  Croatia: "克罗地亚",
  Haiti: "海地",
  Hungary: "匈牙利",
  Indonesia: "印度尼西亚",
  India: "印度",
  Ireland: "爱尔兰",
  Iran: "伊朗",
  Iraq: "伊拉克",
  Iceland: "冰岛",
  Israel: "以色列",
  Italy: "意大利",
  Jamaica: "牙买加",
  Jordan: "约旦",
  Japan: "日本",
  Kazakhstan: "哈萨克斯坦",
  Kenya: "肯尼亚",
  Kyrgyzstan: "吉尔吉斯斯坦",
  Cambodia: "柬埔寨",
  Kosovo: "科索沃",
  Kuwait: "科威特",
  Laos: "老挝",
  Lebanon: "黎巴嫩",
  Liberia: "利比里亚",
  Libya: "利比亚",
  "Sri Lanka": "斯里兰卡",
  Lesotho: "莱索托",
  Lithuania: "立陶宛",
  Luxembourg: "卢森堡",
  Latvia: "拉脱维亚",
  Morocco: "摩洛哥",
  Moldova: "摩尔多瓦",
  Madagascar: "马达加斯加",
  Mexico: "墨西哥",
  Macedonia: "马其顿",
  Mali: "马里",
  Myanmar: "缅甸",
  Montenegro: "黑山",
  Mongolia: "蒙古",
  Mozambique: "莫桑比克",
  Mauritania: "毛里塔尼亚",
  Malawi: "马拉维",
  Malaysia: "马来西亚",
  Namibia: "纳米比亚",
  "New Caledonia": "新喀里多尼亚",
  Niger: "尼日尔",
  Nigeria: "尼日利亚",
  Nicaragua: "尼加拉瓜",
  Netherlands: "荷兰",
  Norway: "挪威",
  Nepal: "尼泊尔",
  "New Zealand": "新西兰",
  Oman: "阿曼",
  Pakistan: "巴基斯坦",
  Panama: "巴拿马",
  Peru: "秘鲁",
  Philippines: "菲律宾",
  "Papua New Guinea": "巴布亚新几内亚",
  Poland: "波兰",
  "Puerto Rico": "波多黎各",
  "North Korea": "北朝鲜",
  Portugal: "葡萄牙",
  Paraguay: "巴拉圭",
  Qatar: "卡塔尔",
  Romania: "罗马尼亚",
  Russia: "俄罗斯",
  Rwanda: "卢旺达",
  "Western Sahara": "西撒哈拉",
  "Saudi Arabia": "沙特阿拉伯",
  Sudan: "苏丹",
  "South Sudan": "南苏丹",
  Senegal: "塞内加尔",
  "Solomon Islands": "所罗门群岛",
  "Sierra Leone": "塞拉利昂",
  "El Salvador": "萨尔瓦多",
  Somaliland: "索马里兰",
  Somalia: "索马里",
  "Republic of Serbia": "塞尔维亚",
  Suriname: "苏里南",
  Slovakia: "斯洛伐克",
  Slovenia: "斯洛文尼亚",
  Sweden: "瑞典",
  Swaziland: "斯威士兰",
  Syria: "叙利亚",
  Chad: "乍得",
  Togo: "多哥",
  Thailand: "泰国",
  Tajikistan: "塔吉克斯坦",
  Turkmenistan: "土库曼斯坦",
  "East Timor": "东帝汶",
  "Trinidad and Tobago": "特里尼达和多巴哥",
  Tunisia: "突尼斯",
  Turkey: "土耳其",
  "United Republic of Tanzania": "坦桑尼亚",
  Uganda: "乌干达",
  Ukraine: "乌克兰",
  Uruguay: "乌拉圭",
  "United States": "美国",
  Uzbekistan: "乌兹别克斯坦",
  Venezuela: "委内瑞拉",
  Vietnam: "越南",
  Vanuatu: "瓦努阿图",
  "West Bank": "西岸",
  Yemen: "也门",
  "South Africa": "南非",
  Zambia: "赞比亚",
  Korea: "韩国",
  Tanzania: "坦桑尼亚",
  Zimbabwe: "津巴布韦",
  Congo: "刚果",
  "Central African Rep.": "中非",
  Serbia: "塞尔维亚",
  "Bosnia and Herz.": "波黑",
  "Czech Rep.": "捷克",
  "W. Sahara": "西撒哈拉",
  "Lao PDR": "老挝",
  "Dem. Rep. Korea": "朝鲜",
  "Falkland Is.": "福克兰群岛",
  "Timor-Leste": "东帝汶",
  "Solomon Is.": "所罗门群岛",
  Palestine: "巴勒斯坦",
  "N. Cyprus": "北塞浦路斯",
  Aland: "奥兰群岛",
  "Fr. S. Antarctic Lands": "法属南半球和南极陆地",
  Mauritius: "毛里求斯",
  Comoros: "科摩罗",
  "Eq. Guinea": "赤道几内亚",
  "Guinea-Bissau": "几内亚比绍",
  "Dominican Rep.": "多米尼加",
  "Saint Lucia": "圣卢西亚",
  Dominica: "多米尼克",
  "Antigua and Barb.": "安提瓜和巴布达",
  "U.S. Virgin Is.": "美国原始岛屿",
  Montserrat: "蒙塞拉特",
  Grenada: "格林纳达",
  Barbados: "巴巴多斯",
  Samoa: "萨摩亚",
  Bahamas: "巴哈马",
  "Cayman Is.": "开曼群岛",
  "Faeroe Is.": "法罗群岛",
  "IsIe of Man": "马恩岛",
  Malta: "马耳他共和国",
  Jersey: "泽西",
  "Cape Verde": "佛得角共和国",
  "Turks and Caicos Is.": "特克斯和凯科斯群岛",
  "Siachen Glacier": "锡亚琴冰川",
  "N. Mariana Is.": "北马里亚纳群岛邦",
  Guam: "关岛",
  Bahrain: "巴林",
  "St. Vin. and Gren.": "圣文森特和格林纳丁斯",
};

const mapCenter = [103.289603, 35.2544921875];

// 浅色系
let borderColor = "#e6e8ed";
let chinaColor = "#7fa0dd";
let worldColor = "rgba(191, 210, 245, 0.5)"; // 是chinaColor的rgb取透明度的值
let hoverColor = "rgba(109, 116, 172, 0.3)";
let textColor = "#243a4e";
let hoverTextColor = "#000";

// 深色系 --切换时记得更换CSS背景色
// let borderColor = "#35486b";
// let chinaColor = "#223558";
// let worldColor = "rgba(46,81,207,.1)"; // 是chinaColor的rgb取透明度的值
// let hoverColor = "#314467";
// let textColor = "#e8f4ff";
// let hoverTextColor = "#fff";

export default {
  name: "ScreenMap",
  data() {
    return {};
  },
  mounted() {
    this.initMap();
  },
  methods: {
    back() {
      this.initMap();
    },
    initMap() {
      let mychart = echarts.init(document.getElementById("mapChart"));
      let backBtn = document.querySelector(".close-back");
      backBtn.style.display = "none";
      let selected = "world";
      drawMap(selected);

      mychart.on("click", function (param) {
        // console.log('param.name', param.name)
        for (let i = 0; i < provincesText.length; i++) {
          if (param.name.includes(provincesText[i])) {
            //显示对应省份的方法
            backBtn.style.display = "block";
            drawMap(provinces[i]);
            break;
          }
        }
      });

      function drawMap(name) {
        let mapJson = "";
        name == "world"
          ? (mapJson = require("@/assets/json/world.json"))
          : (mapJson = require("@/assets/json/province/" + name + ".json"));
        echarts.registerMap(name, mapJson);
        let normalStyle =
          name == "world"
            ? {
                areaColor: worldColor,
                borderColor: borderColor,
                borderWidth: 1,
                shadowBlur: 1, // 图形阴影的模糊大小。
                shadowColor: chinaColor, // 阴影色
                shadowOffsetX: 1, // X轴阴影
                shadowOffsetY: 1, // Y轴阴影
              }
            : {
                borderColor: borderColor, // 图形的描边颜色
                borderWidth: 1, // 描边线宽。
                borderType: "solid", // 柱条的描边类型。
                areaColor: chinaColor, // 图形的颜色
                shadowBlur: 2, // 图形阴影的模糊大小。
                shadowColor: chinaColor, // 阴影色
                shadowOffsetX: 2, // X轴阴影
                shadowOffsetY: 2, // Y轴阴影
              };
        let option = {
          animation: false,
          geo: {
            show: true,
            map: name,
            zoom: 4.5,
            center: mapCenter,
            roam: true,
            label: {
              normal: {
                show: true,
                color: textColor,
              },
              emphasis: {
                color: hoverTextColor,
              },
            },
            itemStyle: {
              normal: normalStyle,
              emphasis: {
                areaColor: hoverColor,
                borderColor: borderColor, // 图形的描边颜色
                borderWidth: "1",
                label: {
                  show: true,
                  textStyle: {
                    color: "#fff",
                    fontSize: 14,
                  },
                },
              },
            },
            nameMap,
            regions: [
              {
                name: "中国",
                itemStyle: {
                  normal: {
                    borderColor: borderColor, // 图形的描边颜色
                    borderWidth: 2, // 描边线宽。
                    borderType: "solid", // 柱条的描边类型。
                    areaColor: chinaColor, // 图形的颜色
                    shadowBlur: 3, // 图形阴影的模糊大小。
                    shadowColor: "rgba(42, 68, 124, 0.6)", // 阴影色
                    shadowOffsetX: 3, // X轴阴影
                    shadowOffsetY: 3, // Y轴阴影
                  },
                },
              },
            ],
          },
          series: [],
        };
        mychart.setOption(option);

        // world 世界地图
        if (name == "world") {
          mychart.setOption({
            geo: {
              center: mapCenter,
              // 设置最小缩放至 & 最大缩放到
              scaleLimit: {
                min: 4.5,
                max: 20,
              },
            },
          });
        } else {
          mychart.setOption({
            geo: {
              center: undefined,
              scaleLimit: {
                min: 0.5,
                max: 10,
              },
              zoom: 1,
            },
          });
        }

        window.addEventListener("resize", () => {
          mychart.resize();
        });
      }
    },
  },
};
</script>
<style lang="css" scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#mapChart {
  width: 100%;
  height: 100%;
}
.container {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  margin: 0 auto;
  background-size: 100% 100%;
  /* 浅色背景 */
  background-color: #e3edfc;
  /* 深色背景 */
  /* background-color: #051023; */
  color: white;
}
.map-container {
  width: 100%;
  height: 100vh;
}
.close-back {
  position: absolute;
  top: 15px;
  right: 100px;
  z-index: 999;
  text-align: center;
  height: 2rem;
  line-height: 2rem;
  border-radius: 0.25rem;
  font-size: 14px;
  width: 120px;
  color: #1398eb;
  border: 1px solid #1398eb;
  background-color: transparent;
  display: none;
  cursor: pointer;
}
</style>
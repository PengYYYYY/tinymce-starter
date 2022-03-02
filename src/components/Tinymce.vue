<template>
  <Editor v-model="innerVal" :init="setting" :disabled="disabled" />
</template>

<script setup lang="ts">
import { ref, onMounted, PropType, watch } from 'vue';
import Editor from '@tinymce/tinymce-vue';
import tinymce from 'tinymce/tinymce'; // tinymce默认hidden，不引入则不显示编辑器

// 引入tinymce编辑器
// https://www.cnblogs.com/huihuihero/p/13877589.html

// 引入node_modules里的tinymce相关文件文件
import 'tinymce/themes/silver'; // 编辑器主题，不引入则报错
import 'tinymce/icons/default'; // 引入编辑器图标icon，不引入则不显示对应图标

// 引入编辑器插件（基本免费插件都在这儿了）
import 'tinymce/plugins/advlist'; // 高级列表
import 'tinymce/plugins/anchor'; // 锚点
import 'tinymce/plugins/autolink'; // 自动链接
import 'tinymce/plugins/autoresize'; // 编辑器高度自适应,注：plugins里引入此插件时，Init里设置的height将失效
import 'tinymce/plugins/autosave'; // 自动存稿
import 'tinymce/plugins/charmap'; // 特殊字符
import 'tinymce/plugins/code'; // 编辑源码
import 'tinymce/plugins/codesample'; // 代码示例
import 'tinymce/plugins/directionality'; // 文字方向
import 'tinymce/plugins/emoticons'; // 表情
import 'tinymce/plugins/fullpage'; // 文档属性
import 'tinymce/plugins/fullscreen'; // 全屏
import 'tinymce/plugins/help'; // 帮助
import 'tinymce/plugins/hr'; // 水平分割线
import 'tinymce/plugins/image'; // 插入编辑图片
import 'tinymce/plugins/importcss'; // 引入css
import 'tinymce/plugins/insertdatetime'; // 插入日期时间
import 'tinymce/plugins/link'; // 超链接
import 'tinymce/plugins/lists'; // 列表插件
import 'tinymce/plugins/media'; // 插入编辑媒体
import 'tinymce/plugins/nonbreaking'; // 插入不间断空格
import 'tinymce/plugins/pagebreak'; // 插入分页符
import 'tinymce/plugins/paste'; // 粘贴插件
import 'tinymce/plugins/preview'; // 预览
import 'tinymce/plugins/print'; // 打印
import 'tinymce/plugins/quickbars'; // 快速工具栏
import 'tinymce/plugins/save'; // 保存
import 'tinymce/plugins/searchreplace'; // 查找替换
// import 'tinymce/plugins/spellchecker'  //拼写检查，暂未加入汉化，不建议使用
import 'tinymce/plugins/tabfocus'; // 切入切出，按tab键切出编辑器，切入页面其他输入框中
import 'tinymce/plugins/table'; // 表格
import 'tinymce/plugins/template'; // 内容模板
import 'tinymce/plugins/textcolor'; // 文字颜色
import 'tinymce/plugins/textpattern'; // 快速排版
import 'tinymce/plugins/toc'; // 目录生成器
import 'tinymce/plugins/visualblocks'; // 显示元素范围
import 'tinymce/plugins/visualchars'; // 显示不可见字符
import 'tinymce/plugins/wordcount'; // 字数统计

const imageUploadHandler = async (setting, blobInfo, success, failure, progress) => {
  const formData = new FormData();

  formData.append('file', blobInfo.blob());
  try {
    // 上传动作，自定义上传
    const fileUrl = ''
    success(fileUrl);
  } catch (error) {
    failure(error);
  }
};

const props = defineProps({
  modelValue: String as PropType<string>,
  height: {
    type: Number as PropType<number>,
    default: 400,
  },
  setting: {
    type: Object,
    default: () => ({}),
  },
  disabled: Boolean as PropType<boolean>,
});

const emit = defineEmits(['update:modelValue']);

const innerVal = ref(props.modelValue);

onMounted(() => {
  tinymce.init({});
});

const setting = {
  language_url: '/tinymce/langs/zh_CN.js', // 引入语言包文件
  language: 'zh_CN', // 语言类型

  skin_url: '/tinymce/skins/ui/oxide', // 皮肤：浅色
  // skin_url: '/tinymce/skins/ui/oxide-dark',//皮肤：暗色

  toolbar_drawer: 'sliding',
  quickbars_selection_toolbar:
    'removeformat | bold italic underline strikethrough | fontsizeselect forecolor backcolor',
  // toolbar:
  //   'undo redo | fullscreen | formatselect alignleft aligncenter alignright alignjustify | link unlink | numlist bullist | image media table | fontsizeselect forecolor backcolor | bold italic underline strikethrough | indent outdent | superscript subscript | removeformat |',
  // plugins: 'code link image media table lists fullscreen quickbars',
  plugins:
    'print preview searchreplace autolink directionality visualblocks visualchars fullscreen image link media template code codesample table charmap hr pagebreak nonbreaking anchor insertdatetime advlist lists wordcount textpattern autosave quickbars', // 插件配置
  toolbar: `
      preview undo redo restoredraft | alignleft aligncenter alignright alignjustify | bullist numlist | forecolor backcolor bold italic underline strikethrough fullscreen |  \
      fontselect fontsizeselect styleselect | outdent indent indent2em lineheight | cut copy paste pastetext print |\
      table image media charmap hr pagebreak insertdatetime | code selectall searchreplace visualblocks link anchor |  blockquote subscript superscript removeformat
    `, // 工具栏配置，设为false则隐藏
  // menubar: 'file edit', // 菜单栏配置，设为false则隐藏，不配置则默认显示全部菜单，也可自定义配置--查看 http://tinymce.ax-z.cn/configure/editor-appearance.php --搜索“自定义菜单”

  // toolbar_groups: {
  //   formatting: {
  //     text: '文字格式',
  //     tooltip: 'Formatting',
  //     items: 'bold italic underline | superscript subscript',
  //   },
  //   alignment: {
  //     icon: 'align-left',
  //     tooltip: 'alignment',
  //     items: 'alignleft aligncenter alignright alignjustify',
  //   },
  // },

  fontsize_formats: '12px 14px 16px 18px 20px 22px 24px 28px 32px 36px 48px 56px 72px', // 字体大小
  font_formats:
    '微软雅黑=Microsoft YaHei,Helvetica Neue,PingFang SC,sans-serif;苹果苹方=PingFang SC,Microsoft YaHei,sans-serif;宋体=simsun,serif;仿宋体=FangSong,serif;黑体=SimHei,sans-serif;Arial=arial,helvetica,sans-serif;Arial Black=arial black,avant garde;Book Antiqua=book antiqua,palatino;', // 字体样式
  lineheight_formats: '0.5 0.8 1 1.2 1.5 1.75 2 2.5 3 4 5', // 行高配置，也可配置成"12px 14px 16px 20px"这种形式

  nonbreaking_force_tab: true,

  height: 400, // 注：引入autoresize插件时，此属性失效
  placeholder: '在这里输入文字',
  branding: true, // tiny技术支持信息是否显示
  resize: true, // 编辑器宽高是否可变，false-否,true-高可变，'both'-宽高均可，注意引号
  statusbar: true, // 最下方的元素路径和字数统计那一栏是否显示
  elementpath: true, // 元素路径是否显示

  toolbar_sticky: true,
  // toolbar_drawer: 'sliding',
  content_style: 'img {max-width:100%;} #tinymce {overflow-x: hidden;}', // 直接自定义可编辑区域的css样式
  // content_css: '/tinycontent.css',  //以css文件方式自定义可编辑区域的css样式，css文件需自己创建并引入

  paste_data_images: true, // 图片是否可粘贴
  images_file_types: 'jpeg,jpg,png,gif,bmp,webp',
  images_upload_handler: (...arg) => {
    imageUploadHandler.apply(null, [props.setting || {}, ...arg]);
  },
};

watch(
  () => innerVal.value,
  () => {
    emit('update:modelValue', innerVal.value);
  },
);

watch(
  () => props.modelValue,
  () => {
    innerVal.value = props.modelValue;
  },
);
</script>

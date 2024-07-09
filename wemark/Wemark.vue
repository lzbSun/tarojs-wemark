<template>
  <view class="wemark_wrapper">
    <block v-if="type === 'wemark'">
      <view v-for="(renderBlock, index) in data.parsedData" :key="index" :class="`wemark_block_${renderBlock.type}`">
        <block v-if="renderBlock.isArray">
          <view v-for="(renderInline, inlineIndex) in renderBlock.content" :key="inlineIndex">
            <text :class="`wemark_inline_${renderInline.type}`"
              v-if="renderInline.type === 'text' || renderInline.type === 'code' || renderInline.type === 'strong' || renderInline.type === 'strong_em' || renderInline.type === 'deleted' || renderInline.type === 'em' || renderInline.type === 'table_th' || renderInline.type === 'table_td'">{{
                renderInline.content }}</text>
            <!-- 代码高亮 -->
            <text class="wemark_inline_code_{{renderInline.type}}" v-if="renderInline.type && renderBlock.highlight">{{
              renderInline.content }}</text>
            <text class="wemark_inline_code_text" v-if="!renderInline.type">{{ renderInline }}</text>
            <navigator class="wemark_inline_link" :url="renderInline.data.href" v-if="renderInline.type === 'link'">{{
              renderInline.content }}</navigator>
            <image mode="widthFix" class="wemark_inline_image" :src="renderInline.src"
              v-if="renderInline.type === 'image'"></image>
          </view>
        </block>
        <block v-if="!renderBlock.isArray">
          <view v-if="renderBlock.type === 'code'">{{ renderBlock.content }}</view>
          <video v-if="renderBlock.type == 'video'" class="wemark_block_video" :src="renderBlock.src"
            :poster="renderBlock.poster" controls></video>
        </block>
      </view>
    </block>
    <rich-text class="wemark_wrapper_richtext" v-if="type === 'rich-text'" :nodes="data.richTextNodes"></rich-text>
  </view>
</template>
<script lang="ts" setup>
import { reactive, watch } from 'vue';
import { parse } from './parser'
import getRichTextNodes from './richtext'
import './index.less'
const props = defineProps<{ md: string, type: string, link: boolean, highlight: boolean }>()
const data = reactive<{ parsedData: any, richTextNodes: any }>({ parsedData: {}, richTextNodes: [] })

const parseMd = () => {
  if (props.md) {
    var parsedData = parse(props.md, {
      link: props.link,
      highlight: props.highlight
    });
    // console.log('parsedData:', parsedData);
    if (props.type === 'wemark') {
      data.parsedData = parsedData

    } else {
      // var inTable = false;
      var richTextNodes = getRichTextNodes(parsedData);

      // console.log('richTextNodes:', richTextNodes);
      data.richTextNodes = richTextNodes as any

    }

  }
}

watch(() => props.md, parseMd, { immediate: true, deep: true })
</script>

<template>
  <AcceptRetrieveTelemetryDialog
    v-model:dialogOpened="isAcceptRetrieveTelemetryDialogOpenComputed"
  />
  <AcceptTermsDialog v-model:dialogOpened="isAcceptTermsDialogOpenComputed" />
  <SettingDialog v-model:dialogOpened="isSettingDialogOpenComputed" />
  <HotkeySettingDialog
    v-model:dialogOpened="isHotkeySettingDialogOpenComputed"
  />
  <ToolBarCustomDialog
    v-model:dialogOpened="isToolbarSettingDialogOpenComputed"
  />
  <CharacterOrderDialog
    v-if="orderedAllCharacterInfos.length > 0"
    v-model:dialogOpened="isCharacterOrderDialogOpenComputed"
    :characterInfos="orderedAllCharacterInfos"
  />
  <DefaultStyleListDialog
    v-if="orderedTalkCharacterInfos.length > 0"
    v-model:dialogOpened="isDefaultStyleSelectDialogOpenComputed"
    :characterInfos="orderedTalkCharacterInfos"
  />
  <DictionaryManageDialog
    v-model:dialogOpened="isDictionaryManageDialogOpenComputed"
  />
  <EngineManageDialog v-model:dialogOpened="isEngineManageDialogOpenComputed" />
  <UpdateNotificationDialogContainer
    :canOpenDialog="canOpenNotificationDialog"
  />
  <ExportSongAudioDialog v-model:dialogOpened="isExportSongAudioDialogOpen" />
  <ImportSongProjectDialog v-model="isImportSongProjectDialogOpenComputed" />
  <PresetManageDialog v-model:dialogOpened="isPresetManageDialogOpenComputed" />
  <HelpDialog v-model:dialogOpened="isHelpDialogOpenComputed" />
</template>

<script setup lang="ts">
import { computed } from "vue";
import SettingDialog from "@/components/Dialog/SettingDialog/SettingDialog.vue";
import HotkeySettingDialog from "@/components/Dialog/HotkeySettingDialog.vue";
import ToolBarCustomDialog from "@/components/Dialog/ToolBarCustomDialog.vue";
import DefaultStyleListDialog from "@/components/Dialog/OldDefaultStyleListDialog.vue";
import CharacterOrderDialog from "@/components/Dialog/OldCharacterOrderDialog.vue";
import AcceptRetrieveTelemetryDialog from "@/components/Dialog/AcceptDialog/AcceptRetrieveTelemetryDialog.vue";
import AcceptTermsDialog from "@/components/Dialog/AcceptDialog/AcceptTermsDialog.vue";
import DictionaryManageDialog from "@/components/Dialog/DictionaryManageDialog.vue";
import EngineManageDialog from "@/components/Dialog/EngineManageDialog.vue";
import UpdateNotificationDialogContainer from "@/components/Dialog/UpdateNotificationDialog/Container.vue";
import ImportSongProjectDialog from "@/components/Dialog/ImportSongProjectDialog.vue";
import ExportSongAudioDialog from "@/components/Dialog/ExportSongAudioDialog/Container.vue";
import PresetManageDialog from "@/components/Dialog/PresetManageDialog.vue";
import HelpDialog from "@/components/Dialog/HelpDialog/HelpDialog.vue";
import { useStore } from "@/store";
import { filterCharacterInfosByStyleType } from "@/store/utility";

const props = defineProps<{
  isEnginesReady: boolean;
}>();
const store = useStore();

// 設定
const isSettingDialogOpenComputed = computed({
  get: () => store.state.isSettingDialogOpen,
  set: (val) => store.actions.SET_DIALOG_OPEN({ isSettingDialogOpen: val }),
});

// ショートカットキー設定
const isHotkeySettingDialogOpenComputed = computed({
  get: () => store.state.isHotkeySettingDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isHotkeySettingDialogOpen: val,
    }),
});

// ツールバーのカスタム設定
const isToolbarSettingDialogOpenComputed = computed({
  get: () => store.state.isToolbarSettingDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isToolbarSettingDialogOpen: val,
    }),
});

// 利用規約表示
const isAcceptTermsDialogOpenComputed = computed({
  get: () => store.state.isAcceptTermsDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isAcceptTermsDialogOpen: val,
    }),
});

// キャラクター並び替え
const orderedAllCharacterInfos = computed(
  () => store.getters.GET_ORDERED_ALL_CHARACTER_INFOS,
);
const isCharacterOrderDialogOpenComputed = computed({
  get: () =>
    !store.state.isAcceptTermsDialogOpen &&
    store.state.isCharacterOrderDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isCharacterOrderDialogOpen: val,
    }),
});

// デフォルトスタイル選択(トーク)
const orderedTalkCharacterInfos = computed(() => {
  return filterCharacterInfosByStyleType(
    store.getters.GET_ORDERED_ALL_CHARACTER_INFOS,
    "talk",
  );
});
const isDefaultStyleSelectDialogOpenComputed = computed({
  get: () =>
    !store.state.isAcceptTermsDialogOpen &&
    !store.state.isCharacterOrderDialogOpen &&
    store.state.isDefaultStyleSelectDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isDefaultStyleSelectDialogOpen: val,
    }),
});

// エンジン管理
const isEngineManageDialogOpenComputed = computed({
  get: () => store.state.isEngineManageDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isEngineManageDialogOpen: val,
    }),
});

// 読み方＆アクセント辞書
const isDictionaryManageDialogOpenComputed = computed({
  get: () => store.state.isDictionaryManageDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isDictionaryManageDialogOpen: val,
    }),
});

const isAcceptRetrieveTelemetryDialogOpenComputed = computed({
  get: () =>
    !store.state.isAcceptTermsDialogOpen &&
    !store.state.isCharacterOrderDialogOpen &&
    !store.state.isDefaultStyleSelectDialogOpen &&
    store.state.isAcceptRetrieveTelemetryDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isAcceptRetrieveTelemetryDialogOpen: val,
    }),
});

// エディタのアップデート確認ダイアログ
const canOpenNotificationDialog = computed(() => {
  return (
    !store.state.isAcceptTermsDialogOpen &&
    !store.state.isCharacterOrderDialogOpen &&
    !store.state.isDefaultStyleSelectDialogOpen &&
    !store.state.isAcceptRetrieveTelemetryDialogOpen &&
    props.isEnginesReady
  );
});

// ソングのオーディオエクスポート時の設定ダイアログ
const isExportSongAudioDialogOpen = computed({
  get: () => store.state.isExportSongAudioDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isExportSongAudioDialogOpen: val,
    }),
});

// ソングのプロジェクトファイルのインポート時の設定ダイアログ
const isImportSongProjectDialogOpenComputed = computed({
  get: () => store.state.isImportSongProjectDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isImportSongProjectDialogOpen: val,
    }),
});

// プリセット管理ダイアログ
const isPresetManageDialogOpenComputed = computed({
  get: () => store.state.isPresetManageDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isPresetManageDialogOpen: val,
    }),
});

// ヘルプダイアログ
const isHelpDialogOpenComputed = computed({
  get: () => store.state.isHelpDialogOpen,
  set: (val) =>
    store.actions.SET_DIALOG_OPEN({
      isHelpDialogOpen: val,
    }),
});
</script>

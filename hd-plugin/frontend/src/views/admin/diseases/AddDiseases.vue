<script setup lang="ts">
import { VNodeChild, h, ref } from 'vue';
import { NForm, NInput, NButton, FormInst, UploadCustomRequestOptions, SelectOption, NAvatar } from 'naive-ui';
import { t } from '@/locales';
import { useMessage, NGrid, NFormItemGi, NUpload, NSwitch, NSelect } from 'naive-ui';
import { useDashboardStore} from '@/store'
import { useLanguage } from '@/hooks/useLanguage'

const { language } = useLanguage()
const dashboardStore = useDashboardStore()
const message = useMessage();
const formRef = ref<FormInst | null>(null);
const loading = ref(false);
const model = ref<Dashboard.Diseases>({
    name: '',
    image_url: '',
    description:'',
    keyLabel:0,
    healthCondition:'',
    languageCodes: ["en", "ar"],
    translationsName: ["", ""],
  translationsDescription: ["", ""],
  translationsHealthCondition: ["", ""],
    status:true,
    createdAt: '',
    updatedAt: '',
  });
  const formData = new FormData();
const rules = {
  name: [{ required: true, message: t('university.nameRequired'), trigger: ['input', 'blur'] }],
  plant: [{ required: true, message: t('university.countryRequired') }],
  image: [{ required: false, message: t('university.imageRequired'), trigger: ['input', 'blur'] }],
  healthCondition: [{ required: true, message: t('dashboard.healthConditionRequired'), trigger: ['input', 'blur'] }],
  description: [{ required: true, message: t('dashboard.descriptionRequired'), trigger: ['input', 'blur'] }],
  keyLabel:[{ required: true, message: t('dashboard.keyLabelRequired'), trigger: ['input', 'blur'] }],
};
const selectOption = dashboardStore.listPlants.map(plant => ({
  label: plant.translations[language.name === 'ar-DZ' ? 1 : 0],
  value: plant.id,
  disabled: false,
}));
async function handleAdded() {
  try {
    loading.value = true;
    formData.append('plantId', model.value?.plantId); 
    formData.append('language_codes', JSON.stringify(model.value.languageCodes));
    formData.append('translations_name', JSON.stringify(model.value.translationsName)); 
    formData.append('translations_description', JSON.stringify(model.value.translationsDescription)); 
    formData.append('translations_health_condition', JSON.stringify(model.value.translationsHealthCondition)); 
    model.value.name = model.value.translationsName[language.name === 'ar-DZ' ? 1 : 0];
    model.value.description = model.value.translationsDescription[language.name === 'ar-DZ' ? 1 : 0];
    model.value.healthCondition = model.value.translationsHealthCondition[language.name === 'ar-DZ' ? 1 : 0];
    // formData.append('name', model.value.name); 
    // formData.append('description', model.value.description); 
    // formData.append('healthCondition', model.value.healthCondition); 
    formData.append('status', model.value.status.toString()); 
    formData.append('keyLabel', model.value.keyLabel); 
    
    await dashboardStore.insertActionDiseases(model.value, formData);
    loading.value = false;
    message.success(t('common.addSuccess'));
    dashboardStore.showModelAddDiseases = false
  } catch (error: any) {
    loading.value = false;
    console.error(t('common.addFailed'), error.message);
    message.error(t('common.addFailed'), error.message);
  }
}

const customRequest = async ({
    file,
    data,
    onProgress,
    onFinish,
    onError
}: UploadCustomRequestOptions) => {
  try {
    
    if (file.file !== null) {
      formData.append('image', file.file); 
      model.value.image = file.file
    }
      onFinish()
  }
  catch (error: any) {
    console.log(error)
    message.error(error.message);
    onError();
  }

}

function isButtonDisabled() {

  //  for (const key in rules) {
  //   if (rules[key].some(rule => rule.required) && !model.value[key]) {
  //     return true;
  //   }
  // }
  return false; 
}

const renderLabel: (option: SelectOption) => VNodeChild = (option) => {
  return h(
    'div',
    {
      style: {
        display: 'flex',
        alignItems: 'center',
      },
    },
    [
      h(NAvatar, {
                  src: dashboardStore.listPlants.find((plant) =>plant.id === option.value)?.image,
                  round: true,
                  size: 22,
                  style: {
                    marginRight: '4px'
                  }
                }),
      h(
        'span',
        {
          style: {
            marginLeft: '8px',
          },
        },
        option.label as string,
      ),
    ],
  );
};
</script>

<template>
  <div class="border-none shadow-none flex flex-col gap-2 p-2 rounded-lg">

    <div class="post-heading mb-1">
      <div class="gtext text-2xl font-bold underlined">{{ t('dashboard.addDiseases') }}</div>
    </div>
    <NForm
      ref="formRef"
      :model="model"
      :rules="rules"
      size="large"
   
    >
      <div>
        <NGrid
          :cols="4"
          :span="24"
          :x-gap="24"
        >

        <NFormItemGi
              :span="12"
              path="plant"
              :label="t('dashboard.plant')"
            >
              <NSelect
                filterable
                trigger="hover"
                v-model:value="model.plantId"
                :options="selectOption"
                :render-label="renderLabel"
              >
                <NButton>{{ t('dashboard.plant') }}</NButton>
              </NSelect>
            </NFormItemGi>

          <NFormItemGi
            :span="12"
            path="image"
            :label="t('university.image')"
          >
            <NUpload
              accept="image/*"
              list-type="image-card"
              :max=1
              path="image"
              :custom-request="customRequest"
            >
            </NUpload>
          </NFormItemGi>
   

            <!-- Loop for language selection -->
  <template v-for="(languageCode, index) in model.languageCodes" :key="index">
    <NFormItemGi
      :span="12"
      :path="`translationsName[${index}]`"
      :label="`${t('dashboard.nameDiseases')} (${languageCode})`"
    >
      <NInput
        v-model:value="model.translationsName[index]"
        :placeholder="`${t('dashboard.nameDiseases')} (${languageCode})`"
        clearable
        :dir="languageCode == 'ar' ? 'rtl' : ''"
        @keydown.enter.prevent
      />
    </NFormItemGi>
    
    <NFormItemGi
      :span="12"
      :path="`translationsDescription[${index}]`"
      :label="`${t('dashboard.desDiseases')} (${languageCode})`"
    >
      <NInput
        v-model:value="model.translationsDescription[index]"
        :placeholder="`${t('dashboard.desDiseases')} (${languageCode})`"
        clearable
        :dir="languageCode == 'ar' ? 'rtl' : ''"
        @keydown.enter.prevent
        type="textarea"
        :autosize="{ minRows: 2, maxRows: 5 }"
      />
    </NFormItemGi>

    <NFormItemGi
      :span="12"
      :path="`translationsHealthCondition[${index}]`"
      :label="`${t('dashboard.healthCondition')} (${languageCode})`"
    >
      <NInput
        v-model:value="model.translationsHealthCondition[index]"
        :placeholder="`${t('dashboard.healthCondition')} (${languageCode})`"
        clearable
        :dir="languageCode == 'ar' ? 'rtl' : ''"
        @keyup.enter="handleAdded"
        type="textarea"
        :autosize="{ minRows: 2, maxRows: 5 }"
      />
    </NFormItemGi>
  </template>

      

          <NFormItemGi
            :span="12"
            path="keyLabel"
            :label="t('dashboard.keyLabel')"
        
          >
            <NInput
              v-model:value="model.keyLabel"
              :placeholder="t('dashboard.keyLabel')"
              clearable
              @keydown.enter.prevent
          
            />
          </NFormItemGi>

      


          <NFormItemGi
            :span="12"
            path="state"
            :label="t('university.state')"
          >
            <NSwitch
              v-model:value="model.status"
              size="large"
            />
          </NFormItemGi>

        </NGrid>
      </div>

      <div style="display: flex; justify-content: flex-end">
        <NButton
          type="primary"
          style="width:100%;"
          size="large"
          :loading="loading"
          :disabled="isButtonDisabled()"
          @click="handleAdded"
        >
          {{ t('dashboard.addDisease') }}
        </NButton>
      </div>

    </NForm>
  </div>
</template>
  

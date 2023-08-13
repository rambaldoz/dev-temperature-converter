<template>
    <section class="main-content h-screen m-auto flex flex-col align-center">
        <div class="app-title mt-10 mb-2 flex justify-center">
            <h2 class="text-xl font-bold">Temperature Converter</h2>
        </div>
        <div class="content-holder w-1/3 mt-2 mb-4 mx-auto p-8 bg-gray-50 rounded border-b-2 border-slate-300">
            <div class="input-holder">
                <Form 
                    @submit="handleConvert"
                    class="flex flex-col">
                    <div class="inputs grid grid-cols-2">
                        <div class="mx-4 flex flex-col">
                            <label for="input-temp" class="mb-2 text-slate-500 text-sm">Temperature</label>
                            <Field 
                                v-model="inputTemp" 
                                :rules="validateInput"
                                name="input-temp" type="text" id="input-temp" class="h-10 p-2  border rounded" 
                            />
                            <ErrorMessage 
                                name="input-temp" 
                                class="mt-2 text-sm text-red-500"
                            />
                        </div>
                        <div class="mx-4 flex flex-col">
                            <label for="select-temp" class="mb-2 text-slate-500 text-sm">Convert To</label>
                            <Field 
                                v-model="selectType"
                                as="select"
                                :rules="validateTemp"
                                name="select-temp" id="select-temp" class="h-10 p-2 border rounded">
                                <option selected disabled>Select</option>
                                <option value="c">Celsius</option>
                                <option value="f">Farenheit</option>
                            </Field>
                            <ErrorMessage 
                                name="select-temp" 
                                class="mt-2 text-sm text-red-500"
                            />
                        </div>
                    </div>
                    <div class="mt-6 mx-4 flex">
                        <button class="w-full h-14 self-end font-bold text-gray-50 bg-red-600 hover:bg-red-700 rounded">Convert
                        </button>
                    </div>
                </Form>
            </div>
            <div class="input-result mt-6 grid grid-cols-1">
                <div class="mx-4 flex flex-col justify-center text-center">
                    <p class="mb-2 text-slate-500 text-sm">Result</p>
                    <h2 class="font-bold text-xl">{{ resultTemp }}</h2>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
import { ref } from 'vue';
import { Form, Field, ErrorMessage } from 'vee-validate';

const inputTemp = ref('');
const selectType = ref('Select');
const calculationResult = ref('');
const resultTemp = ref('');

const convertToC = () => {
    return calculationResult.value = (inputTemp.value - 32) * 5/9;
}

const convertToF = () => {
    return calculationResult.value = (inputTemp.value * 9/5) + 32;
}

const validateInput = (value) => {
    if (!value) {
        return 'This field is required';
    }
    // if the field is not a valid email
    const regex = /^[0-9.+-]/i;
        if (!regex.test(value)) {
            return 'Invalid value';
        }
    // All is good
    return true;
}

const validateTemp = (value) => {
    if (value === 'Select') {
        return 'Select Temperature';
    }
    // All is good
    return true;
}

const handleConvert = () => {
    if (selectType.value === 'c') {
        resultTemp.value = `${convertToC().toFixed(2)} °C`;
    } else if (selectType.value === 'f') {
        resultTemp.value = `${convertToF().toFixed(2)} °F`;
    } else {
        resultTemp.value = 'Cannot convert.'
    }
}

</script>
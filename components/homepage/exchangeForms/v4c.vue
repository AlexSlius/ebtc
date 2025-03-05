<template>
    <div v-if="info" class="v4c-wrapper">
        <div class="container v4c-cont-position">
            <div class="v4c-row">
                <div class="v4c-left">
                    <h1 v-if="matchedRoute.routeId && !!from.name && !!to.name" class="v4c-title">
                        {{ $t("page.main.details.exchange") }} {{ from.name }} {{ $t("page.main.details.for") }} {{
                            to.name }}</h1>
                    <div class="v4c-left_desc">
                        <div v-if="!matchedRoute.routeId" class="dataInput v4c-cont-inp">
                            <div>
                                <div class="help_block">
                                    <span class="file_icon">
                                        <img src="~assets/img/exchange.svg" alt="Money" />
                                    </span>
                                    <div>
                                        <h3>{{ $t("page.main.details.pleaseSelectRoute") }}</h3>
                                        <p>{{ $t("page.main.details.select_p_1") }}</p>
                                        <p>{{ $t("page.main.details.select_p_2") }}</p>
                                    </div>
                                    <span class="arrow_icon">
                                        <img src="~assets/img/arr_green.svg" alt />
                                    </span>
                                </div>
                            </div>
                        </div>
                        <div v-else-if="errorloadRouteGetOne" class="dataInput">
                            <div class="form--title">
                                <h4>{{ $t("page.main.details.title") }}</h4>
                            </div>
                            <div>
                                <div class="help_block">
                                    <h3>{{ $t("page.main.details.errorRoute") }}</h3>
                                    <p>{{ $t("page.main.details.error_p_1") }}</p>
                                    <p>{{ $t("page.main.details.error_p_2") }}</p>
                                </div>
                            </div>
                        </div>
                        <div v-else>
                            <div v-if="matchedRoute.routeId && !errorloadRouteGetOne && instructions?.length > 3"
                                class="v4c-left_warning">{{
                                    $t("page.main.details.before_text") }}</div>
                            <div class="v4c-left_desc_main">
                                <p v-for="instruction in instructions" :key="instruction._id"
                                    v-html="instruction.content" />
                            </div>
                        </div>
                    </div>
                </div>
                <div class="v4c-calc">
                    <form class="v4c-form">
                        <div class="v4c-form_wr">
                            <div class="v4c-form-first-stap">
                                <div class="v4c-form_tow">
                                    <div class="v4c-form_tow-left">
                                        <img src="~assets/img/form-left-right.svg" alt="">
                                        <div class="v4c-form_tow-left-des">
                                            <div class="v4c-for_row_t">{{ $t("page.main.details.converter") }}</div>
                                            <p class="v4c-for_row_d">{{ from.symbol }} > {{ to.symbol }}</p>
                                        </div>
                                    </div>
                                    <div class="v4c-form_tow-right">
                                        <div class="v4c-form_tow-right_rate">{{ $t("page.main.to.rate") }}</div>
                                        <div class="v4c-form_tow-right_val">
                                            <span v-if="rateIn > 20">{{ Number((+rateIn || 1).toFixed(2)) }}</span>
                                            <span v-else-if="rateIn > 10">
                                                {{ Number((+rateIn || 1).toFixed(3)) }}
                                            </span>
                                            <span v-else>{{ Number((+rateIn || 1).toFixed(4)) }}</span>
                                            {{ from.symbol }} =
                                            <span v-if="rateOut > 20">{{ Number((+rateOut || 1).toFixed(2)) }}</span>
                                            <span v-else-if="rateOut > 10">
                                                {{ Number((+rateOut || 1).toFixed(3)) }}
                                            </span>
                                            <span v-else>{{ Number((+rateOut || 1).toFixed(4)) }}</span>
                                            {{ to.symbol }}
                                        </div>
                                    </div>
                                </div>

                                <!-- coins -->
                                <div v-if="!formStapTwo" class="v4c-form_fields">
                                    <!-- from -->
                                    <div :class="['v4c-form_field v4c-form_field_first', { errores: errorFilesMon }]">
                                        <div class="v4c-form-f_top">
                                            <div class="v4c-form-f_top-title">{{ $t("page.main.from.title") }}</div>
                                            <div class="v4c-form-f_top-price">
                                                <div class="v4c-form-f_item"
                                                    v-if="currentRoute.from.min && currentRoute.from.min !== '0'"
                                                    :style="{ color: minAmount ? 'red' : '' }"
                                                    @click="val1 = fixedNumber('ceil', currentRoute.from.decimal, currentRoute.from.min); correctTo()">
                                                    {{ $t("page.main.from.min") }}
                                                    <span :style="{ color: minAmount ? 'red' : '' }">{{
                                                        fixedNumber("ceil", currentRoute.from.decimal,
                                                            currentRoute.from.min) }} </span>
                                                </div>
                                                <div class="v4c-form-f_def">-</div>
                                                <div class="v4c-form-f_item"
                                                    v-if="currentRoute.from.max && currentRoute.from.max !== '0'"
                                                    :style="{ color: maxAmount ? 'red' : '' }"
                                                    @click="val1 = fixedNumber('floor', currentRoute.from.decimal, currentRoute.from.max); correctTo()">
                                                    {{ $t("page.main.from.max") }}
                                                    <span :style="{ color: maxAmount ? 'red' : '' }">{{
                                                        fixedNumber("floor", currentRoute.from.decimal,
                                                            currentRoute.from.max) }}</span>
                                                </div>
                                            </div>
                                        </div>

                                        <div :class="['v4c-form-row_file']">
                                            <input :class="['v4c-form-intput']" v-model="val1"
                                                :style="{ color: minAmount || maxAmount || errorFilesMon ? 'red' : '' }"
                                                type="number" placeholder="0.00" @input="correctTo" />

                                            <div class="v4c-form-type" @click="openModalFrom">
                                                <div v-if="from.image" class="v4c-form-type_img">
                                                    <img :alt="from.name" :src="$rest.urlImg(from.image).small" />
                                                </div>

                                                <div class="v4c-type-vale">
                                                    <div class="v4c-type-vale_name">{{ from.symbol }}</div>
                                                    <div class="v4c-type-vale_des">{{ from.name }}</div>
                                                </div>
                                                <i class="v4c-type-icon">
                                                    <svg xmlns="http://www.w3.org/2000/svg" width="21" height="21"
                                                        viewBox="0 0 21 21" fill="none">
                                                        <circle cx="10.5" cy="10.5" r="10.5" fill="#3B406D" />
                                                        <path d="M7 9L10.5 12L14 9" stroke="white"
                                                            stroke-linecap="round" />
                                                    </svg>
                                                </i>
                                            </div>
                                        </div>
                                    </div>

                                    <!--from cities-->
                                    <div v-if="fromCities && fromCities.length"
                                        class="v4c-fiel-type-select form-v2--input-enter-form">
                                        <select v-model="from" :placeholder="'From city'">
                                            <option v-for="city in fromCities" :key="city.xml" :value="city">
                                                {{ city.cityName }}
                                            </option>
                                        </select>
                                    </div>

                                    <!-- reverce -->
                                    <div class="v4c-form_field_reverce">
                                        <button type="button" class="v4c-form_field-bnt-reverce"
                                            @click="reverseCurrency">
                                            <svg width="18" height="16" viewBox="0 0 18 16" fill="none"
                                                xmlns="http://www.w3.org/2000/svg">
                                                <path d="M10.8672 1.61377V14.3862L16.0253 9.22816" fill="none"
                                                    stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                                                <path d="M7.125 14.3862V1.61377L1.96693 6.77184" fill="none"
                                                    stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                                            </svg>
                                        </button>
                                    </div>

                                    <!-- to -->
                                    <div :class="['v4c-form_field', { errores: errorFilesMon }]">
                                        <div class="v4c-form-f_top">
                                            <div class="v4c-form-f_top-title">{{ $t("page.main.to.title") }}</div>
                                            <!-- <div class="v4c-form-f_top-price">
                                                <div class="v4c-form-f_item"
                                                    v-if="currentRoute && currentRoute.rate && currentRoute.rate.amount"
                                                    :style="{ color: limitAmount ? 'red' : '' }">{{
                                                        $t("page.main.to.reserve") }} <span
                                                        :style="{ color: limitAmount ? 'red' : '' }">{{
                                                            fixedNumber("floor",
                                                                currentRoute.to.decimal, currentRoute.rate.amount) }}</span>
                                                </div>
                                            </div> -->
                                        </div>
                                        <div :class="['v4c-form-row_file']">
                                            <input :class="['v4c-form-intput', 'v4c-form-intput_to']" v-model="val2"
                                                :style="{ color: limitAmount || errorFilesMon ? 'red' : '' }"
                                                type="number" placeholder="0.00" @input="correctFrom" />
                                            <div class="v4c-form-type" @click="openModalTo">
                                                <div v-if="to.image" class="v4c-form-type_img">
                                                    <img :alt="to.name" :src="$rest.urlImg(to.image).small" />
                                                </div>
                                                <div class="v4c-type-vale">
                                                    <div class="v4c-type-vale_name">{{ to.symbol }}</div>
                                                    <div class="v4c-type-vale_des">{{ to.name }}</div>
                                                </div>
                                                <i class="v4c-type-icon">
                                                    <svg xmlns="http://www.w3.org/2000/svg" width="21" height="21"
                                                        viewBox="0 0 21 21" fill="none">
                                                        <circle cx="10.5" cy="10.5" r="10.5" fill="#3B406D" />
                                                        <path d="M7 9L10.5 12L14 9" stroke="white"
                                                            stroke-linecap="round" />
                                                    </svg>
                                                </i>
                                            </div>
                                        </div>
                                    </div>

                                    <!-- to city -->
                                    <div v-if="toCities && toCities.length"
                                        class="v4c-fiel-type-select form-v2--input-enter-form v4c-form_field-to-city">
                                        <select v-model="to" :placeholder="'From city'">
                                            <option v-for="city in toCities" :key="city.xml" :value="city">
                                                {{ city.cityName }}
                                            </option>
                                        </select>
                                    </div>

                                    <!--check cities-->
                                    <div class="v4c-mt-cit-w">
                                        <p v-if="currentRoute.verification || currentRoute.requiredVerificationUser && currentRoute.requiredVerificationUser.enable"
                                            style="color: #4bb56b;padding-bottom: 10px;">
                                            {{ $t("page.main.details.needVerifPerson") + " " }}
                                        </p>

                                        <p v-if="currentRoute.from.verification"
                                            style="color: #4bb56b;padding-bottom: 10px;">
                                            {{ $t("page.main.details.needVerifCurrency") + " " }}
                                            {{ currentRoute.from.name + " " + currentRoute.from.symbol }}
                                        </p>

                                        <p v-if="currentRoute.to.verificationPayout"
                                            style="color: #4bb56b;padding-bottom: 10px;">
                                            {{ $t("page.main.details.needVerifCurrency") + " " }}
                                            {{ currentRoute.to.name + " " + currentRoute.to.symbol }}
                                        </p>
                                    </div>
                                </div>

                                <!-- personal -->
                                <div v-if="formStapTwo" class="v4c-form_fields v4c-fields-data">
                                    <div class="v4c-field-data-item" v-for="field in currentRoute.from.fields"
                                        :key="field.id">
                                        <input v-model="fromValues[field._id]" :style="{
                                            border: !fromValues[field._id] || fromValues[field._id].match(field.regexp) ? '' : '1px solid red'
                                        }" :placeholder="field.name" type="text" />
                                        <img :src="$rest.urlImg(currentRoute.from.image).small" alt="wallet" />
                                    </div>

                                    <div v-for="field in currentRoute.to.fields" :key="field.id"
                                        class="v4c-field-data-item">
                                        <input v-model="toValues[field._id]" :style="{
                                            border: !toValues[field._id] || toValues[field._id].match(field.regexp) ? '' : '1px solid red'
                                        }" :placeholder="field.name" type="text" />
                                        <img :src="$rest.urlImg(currentRoute.to.image).small" alt="wallet" />
                                    </div>

                                    <div v-for="field in currentRoute.fields" :key="field.id"
                                        class="v4c-field-data-item">
                                        <input v-model="routeValues[field._id]" :style="{
                                            border: !routeValues[field._id] || routeValues[field._id].match(field.regexp) ? '' : '1px solid red'
                                        }" :placeholder="field.name" type="text" />
                                        <img />
                                    </div>
                                </div>

                                <!-- polit -->
                                <div v-if="formStapTwo" class="v4c-form-polit">
                                    <div class="v4c-form_chech">
                                        <p>{{ $t("page.main.details.agree") }}
                                            <nuxt-link :to="localePath('/rules/')">{{ $t("page.main.details.agree_link")
                                                }}</nuxt-link>
                                        </p>
                                        <label class="v4c-chec">
                                            <input type="checkbox" v-model="agree" class="v4c-chec_in">
                                            <i class="v4c-chec_i"></i>
                                        </label>
                                    </div>
                                    <div class="v4c-form_chech">
                                        <p>{{ $t("page.main.details.agree_kyc") }}
                                            <nuxt-link :to="localePath('/rules/aml-kyc-policy/')">{{
                                                $t("page.main.details.agree_link_kyc") }}</nuxt-link>
                                        </p>
                                        <label class="v4c-chec">
                                            <input type="checkbox" v-model="agree_kyc" class="v4c-chec_in">
                                            <i class="v4c-chec_i"></i>
                                        </label>
                                    </div>
                                </div>

                                <!-- loader -->
                                <loader v-if="!load" :responsive="true" width="100" height="100" />

                                <!-- buttons -->
                                <div v-else class="v4c-form_bot">
                                    <button v-if="formStapTwo" class="v4c-form-btn-back"
                                        @click.prevent="formStapTwo = false">
                                        <svg width="24" height="24" viewBox="0 0 24 24" fill="none"
                                            xmlns="http://www.w3.org/2000/svg">
                                            <path d="M3 12H21M21 12L12.5 3.5M21 12L12.5 20.5" stroke="white"
                                                stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
                                            </path>
                                        </svg>
                                    </button>
                                    <button :style="{ opacity: currentRoute.routeId ? 1 : 0.5 }" :disabled="!currentRoute.routeId"  class="v4c-form_submit"
                                        @click.prevent="handleCreate">
                                        <svg data-v-57b367c6="" width="24" height="24" viewBox="0 0 24 24" fill="none"
                                            xmlns="http://www.w3.org/2000/svg">
                                            <path data-v-57b367c6=""
                                                d="M9.01894 9C9.00639 8.83498 9 8.66824 9 8.5C9 4.91015 11.9101 2 15.5 2C19.0899 2 22 4.91015 22 8.5C22 12.0899 19.0899 15 15.5 15C15.3318 15 15.165 14.9936 15 14.9811"
                                                stroke="#ffffff" stroke-width="1.8" stroke-linecap="round"
                                                stroke-linejoin="round"></path>
                                            <path data-v-57b367c6=""
                                                d="M8.5 22C4.91015 22 2 19.0899 2 15.5C2 11.9101 4.91015 9 8.5 9C12.0899 9 15 11.9101 15 15.5C15 19.0899 12.0899 22 8.5 22Z"
                                                stroke="#ffffff" stroke-width="1.8" stroke-linecap="round"
                                                stroke-linejoin="round"></path>
                                            <path data-v-57b367c6=""
                                                d="M22 17C22 18.6569 20.6569 20 19 20H17M17 20L19 18M17 20L19 22"
                                                stroke="#ffffff" stroke-width="1.8" stroke-linecap="round"
                                                stroke-linejoin="round"></path>
                                            <path data-v-57b367c6="" d="M2 7C2 5.34315 3.34315 4 5 4H7M7 4L5 6M7 4L5 2"
                                                stroke="#ffffff" stroke-width="1.8" stroke-linecap="round"
                                                stroke-linejoin="round"></path>
                                        </svg>
                                        <span>{{ $t("page.main.details.submit") }}</span>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </form>
                </div>
            </div>

            <div v-if="showModalFrom" @click="closeModal" class="v4c-modal-after_bot-opas"></div>
            <div v-if="showModalFrom" class="v4c-modal">
                <div class="v4c-modal_cont">
                    <div class="v4c-modal-top">
                        <div class="v4c-modal-title">Виберіть валюту</div>
                        <button @click="closeModal" class="v4c-modal-close">
                            <svg xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 10 10"
                                fill="none">
                                <g clip-path="url(#clip0_0_777)">
                                    <path
                                        d="M6.64999 5.00031L9.74362 1.90668C10.0854 1.56487 10.0854 1.0107 9.74362 0.669421L9.3312 0.257001C8.98928 -0.0849194 8.43511 -0.0849194 8.09383 0.257001L5.00031 3.35052L1.90668 0.25636C1.56487 -0.0854535 1.0107 -0.0854535 0.669421 0.25636L0.25636 0.66878C-0.0854535 1.0107 -0.0854535 1.56487 0.25636 1.90615L3.35052 5.00031L0.257001 8.09383C-0.0849194 8.43575 -0.0849194 8.98992 0.257001 9.3312L0.669421 9.74362C1.01124 10.0854 1.5654 10.0854 1.90668 9.74362L5.00031 6.64999L8.09383 9.74362C8.43575 10.0854 8.98992 10.0854 9.3312 9.74362L9.74362 9.3312C10.0854 8.98928 10.0854 8.43511 9.74362 8.09383L6.64999 5.00031Z"
                                        fill="#0c0c0c" />
                                </g>
                                <defs>
                                    <clipPath id="clip0_0_777">
                                        <rect width="10" height="10" fill="#0c0c0c" />
                                    </clipPath>
                                </defs>
                            </svg>
                        </button>
                    </div>
                    <div class="v4c-modal-wr-searhc">
                        <label class="v4c-moda-label-searhc">
                            <input type="text" placeholder="Пошук валюти">
                            <svg width="28" height="28" viewBox="0 0 28 28" fill="none"
                                xmlns="http://www.w3.org/2000/svg" data-v-57b367c6="">
                                <path
                                    d="M24.1617 22.505L20.195 18.55C21.4748 16.9195 22.1692 14.9061 22.1667 12.8333C22.1667 10.9874 21.6193 9.18287 20.5937 7.64802C19.5682 6.11316 18.1105 4.91688 16.405 4.21046C14.6996 3.50404 12.823 3.31921 11.0125 3.67934C9.20201 4.03947 7.53896 4.92838 6.23367 6.23367C4.92838 7.53896 4.03947 9.20201 3.67934 11.0125C3.31921 12.823 3.50404 14.6996 4.21046 16.405C4.91688 18.1105 6.11316 19.5682 7.64802 20.5937C9.18287 21.6193 10.9874 22.1667 12.8333 22.1667C14.9061 22.1692 16.9195 21.4748 18.55 20.195L22.505 24.1617C22.6135 24.271 22.7425 24.3578 22.8847 24.417C23.0268 24.4763 23.1793 24.5068 23.3333 24.5068C23.4874 24.5068 23.6398 24.4763 23.782 24.417C23.9242 24.3578 24.0532 24.271 24.1617 24.1617C24.271 24.0532 24.3578 23.9242 24.417 23.782C24.4763 23.6398 24.5068 23.4874 24.5068 23.3333C24.5068 23.1793 24.4763 23.0268 24.417 22.8847C24.3578 22.7425 24.271 22.6135 24.1617 22.505ZM5.83334 12.8333C5.83334 11.4489 6.24388 10.0955 7.01305 8.94435C7.78222 7.7932 8.87547 6.89599 10.1546 6.36618C11.4336 5.83637 12.8411 5.69774 14.199 5.96784C15.5568 6.23794 16.8041 6.90462 17.7831 7.88359C18.7621 8.86256 19.4287 10.1098 19.6988 11.4677C19.9689 12.8256 19.8303 14.233 19.3005 15.5121C18.7707 16.7912 17.8735 17.8845 16.7223 18.6536C15.5712 19.4228 14.2178 19.8333 12.8333 19.8333C10.9768 19.8333 9.19634 19.0958 7.88359 17.7831C6.57084 16.4703 5.83334 14.6899 5.83334 12.8333Z"
                                    fill="#1B1918" data-v-57b367c6=""></path>
                            </svg>
                        </label>
                    </div>
                    <div class="v4c-modal-tabs">
                        <button :class="['v4c-modal-item-tab', { active: symbolFrom === '' }]" @click="symbolFrom = ''">{{
                            $t("page.main.from.all")
                        }}</button>
                        <button :class="['v4c-modal-item-tab', { active: symbolFrom === '>USD' }]"
                            @click="symbolFrom = '>USD'">USD</button>
                        <button :class="['v4c-modal-item-tab', { active: symbolFrom === 'EUR' }]"
                            @click="symbolFrom = 'EUR'">EUR</button>
                        <button :class="['v4c-modal-item-tab', { active: symbolFrom === 'UAH' }]"
                            @click="symbolFrom = 'UAH'">UAH</button>
                    </div>
                    <div class="v4c-modal-wr-list">
                        <div v-for="ro in fromListNoCity" :key="ro.routeId" @click="handleClickFrom(ro.from)"
                            :class="['v4c-mod-item-coin', { active: from && ro.from && from.xml === ro.from.xml }]">
                            <div class="v4c-mod-item-coin_left">
                                <div class="v4c-mod-item-coin_left_img">
                                    <img :src="$rest.urlImg(ro.from.image).small" :alt="ro.from.name" />
                                </div>
                                <span>{{ ro.from.name }}</span>
                            </div>
                            <div class="v4c-mod-item-coin_currency">{{ ro.from.symbol }}</div>
                            <span class="cur--check">
                                <img v-if="from && ro.from && from.xml === ro.from.xml"
                                    src="~assets/img/tick-inside-circle.svg" alt="+" />
                            </span>
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="showModalTo" @click="closeModal" class="v4c-modal-after_bot-opas"></div>
            <div v-if="showModalTo" class="v4c-modal">
                <div class="v4c-modal_cont">
                    <div class="v4c-modal-top">
                        <div class="v4c-modal-title">Виберіть валюту</div>
                        <button @click="closeModal" class="v4c-modal-close">
                            <svg xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 10 10"
                                fill="none">
                                <g clip-path="url(#clip0_0_777)">
                                    <path
                                        d="M6.64999 5.00031L9.74362 1.90668C10.0854 1.56487 10.0854 1.0107 9.74362 0.669421L9.3312 0.257001C8.98928 -0.0849194 8.43511 -0.0849194 8.09383 0.257001L5.00031 3.35052L1.90668 0.25636C1.56487 -0.0854535 1.0107 -0.0854535 0.669421 0.25636L0.25636 0.66878C-0.0854535 1.0107 -0.0854535 1.56487 0.25636 1.90615L3.35052 5.00031L0.257001 8.09383C-0.0849194 8.43575 -0.0849194 8.98992 0.257001 9.3312L0.669421 9.74362C1.01124 10.0854 1.5654 10.0854 1.90668 9.74362L5.00031 6.64999L8.09383 9.74362C8.43575 10.0854 8.98992 10.0854 9.3312 9.74362L9.74362 9.3312C10.0854 8.98928 10.0854 8.43511 9.74362 8.09383L6.64999 5.00031Z"
                                        fill="#0c0c0c" />
                                </g>
                                <defs>
                                    <clipPath id="clip0_0_777">
                                        <rect width="10" height="10" fill="#0c0c0c" />
                                    </clipPath>
                                </defs>
                            </svg>
                        </button>
                    </div>
                    <div class="v4c-modal-wr-searhc">
                        <label class="v4c-moda-label-searhc">
                            <input type="text" placeholder="Пошук валюти">
                            <svg width="28" height="28" viewBox="0 0 28 28" fill="none"
                                xmlns="http://www.w3.org/2000/svg" data-v-57b367c6="">
                                <path
                                    d="M24.1617 22.505L20.195 18.55C21.4748 16.9195 22.1692 14.9061 22.1667 12.8333C22.1667 10.9874 21.6193 9.18287 20.5937 7.64802C19.5682 6.11316 18.1105 4.91688 16.405 4.21046C14.6996 3.50404 12.823 3.31921 11.0125 3.67934C9.20201 4.03947 7.53896 4.92838 6.23367 6.23367C4.92838 7.53896 4.03947 9.20201 3.67934 11.0125C3.31921 12.823 3.50404 14.6996 4.21046 16.405C4.91688 18.1105 6.11316 19.5682 7.64802 20.5937C9.18287 21.6193 10.9874 22.1667 12.8333 22.1667C14.9061 22.1692 16.9195 21.4748 18.55 20.195L22.505 24.1617C22.6135 24.271 22.7425 24.3578 22.8847 24.417C23.0268 24.4763 23.1793 24.5068 23.3333 24.5068C23.4874 24.5068 23.6398 24.4763 23.782 24.417C23.9242 24.3578 24.0532 24.271 24.1617 24.1617C24.271 24.0532 24.3578 23.9242 24.417 23.782C24.4763 23.6398 24.5068 23.4874 24.5068 23.3333C24.5068 23.1793 24.4763 23.0268 24.417 22.8847C24.3578 22.7425 24.271 22.6135 24.1617 22.505ZM5.83334 12.8333C5.83334 11.4489 6.24388 10.0955 7.01305 8.94435C7.78222 7.7932 8.87547 6.89599 10.1546 6.36618C11.4336 5.83637 12.8411 5.69774 14.199 5.96784C15.5568 6.23794 16.8041 6.90462 17.7831 7.88359C18.7621 8.86256 19.4287 10.1098 19.6988 11.4677C19.9689 12.8256 19.8303 14.233 19.3005 15.5121C18.7707 16.7912 17.8735 17.8845 16.7223 18.6536C15.5712 19.4228 14.2178 19.8333 12.8333 19.8333C10.9768 19.8333 9.19634 19.0958 7.88359 17.7831C6.57084 16.4703 5.83334 14.6899 5.83334 12.8333Z"
                                    fill="#1B1918" data-v-57b367c6=""></path>
                            </svg>
                        </label>
                    </div>
                    <div class="v4c-modal-tabs">
                        <button :class="['v4c-modal-item-tab', { active: symbolTo === '' }]" @click="symbolTo = ''">{{
                            $t("page.main.to.all") }}</button>
                        <button :class="['v4c-modal-item-tab', { active: symbolTo === 'USD' }]"
                            @click="symbolTo = 'USD'">USD</button>
                        <button :class="['v4c-modal-item-tab', { active: symbolTo === 'EUR' }]"
                            @click="symbolTo = 'EUR'">EUR</button>
                        <button :class="['v4c-modal-item-tab', { active: symbolTo === 'UAH' }]"
                            @click="symbolTo = 'UAH'">UAH</button>
                    </div>
                    <div class="v4c-modal-wr-list">
                        <div v-for="ro in toListNoCity" :key="ro.routeId" @click="handleClickTo(ro.to)"
                            :class="['v4c-mod-item-coin', { active: to && ro.to && to.xml === ro.to.xml }]">
                            <div class="v4c-mod-item-coin_left">
                                <div class="v4c-mod-item-coin_left_img">
                                    <img :alt="ro.to.name" :src="$rest.urlImg(ro.to.image).small" />
                                </div>
                                <span>{{ ro.to.name }}</span>
                            </div>
                            <div class="v4c-mod-item-coin_currency">{{ ro.to.symbol }}</div>
                            <span class="cur--check">
                                <img v-if="to && ro.to && to.xml === ro.to.xml" src="~assets/img/tick-inside-circle.svg"
                                    alt="+" />
                            </span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { mapActions, mapGetters, mapMutations } from "vuex";
import { BigNumber } from "bignumber.js";

export default {
    props: {
        isWidget: {
            type: Boolean,
            default: false
        },
        widgetFrom: { type: String, default: () => "" },
        widgetTo: { type: String, default: () => "" },
        widgetAmount: { type: [String, Number], default: () => "" },
        widgetWidth: { type: [String, Number], default: () => "" }
    },
    data() {
        return {
            loadRouteGetOne: false,
            errorloadRouteGetOne: false,
            limitAmount: false,
            minAmount: false,
            maxAmount: false,
            sendAmount: "in", // in - user want take some money, out - get some money
            errorRoutes: "",
            from: "",
            to: "",
            val1: "",
            val2: "",
            amount: "",
            captcha: "",
            fromValues: {},
            toValues: {},
            routeValues: {},
            email: "",
            symbolFrom: "",
            symbolTo: "",
            valid: 1,
            agree: false,
            agree_kyc: false,
            currentRoute: {
                to: {},
                from: {},
                rate: {}
            },
            showModalFrom: false,
            showModalTo: false,
            formStapTwo: false,
            errorFilesMon: false,
        };
    },
    computed: {
        ...mapGetters({
            captchaSettings: "params/getCaptchaConf",
            info: "exchange/info",
            load: "exchange/load",
            profile: "user/info",
        }),
        rateIn() {
            if (!this.currentRoute || !this.currentRoute.rate || !this.currentRoute.rate.in) return 0;
            let rate = new BigNumber(this.currentRoute.rate.in);
            let discountPercent = 0;
            const discount = this.currentRoute.from.discounts.find(el => Number(el.amountMoreThan) <= this.val1);
            if (discount && discount.discountPercent) {
                discountPercent = Number(discount.discountPercent);
            }
            rate = rate.minus(
                new BigNumber(rate)
                    .div(100)
                    .multipliedBy(discountPercent)
            );
            return rate;
        },
        rateOut() {
            if (!this.currentRoute || !this.currentRoute.rate || !this.currentRoute.rate.out) return 0;

            let rate = new BigNumber(this.currentRoute.rate.out);

            return rate;
        },
        fromListNoCity() {
            const direction = 'from';

            return this[direction + "List"]
                .filter((el, indexId) => {
                    const isCash = el[direction].isCash
                    if (isCash) {
                        return (indexId === this[direction + "List"].findIndex(item => item[direction].xml.indexOf(el[direction].xml.split("_C_")[0]) === 0))
                    }
                    return true;
                });
        },
        fromCities() {
            const direction = 'from';

            if (!this || !this[direction] || !this[direction].isCash) {
                return [];
            }

            return this[direction + "List"]
                .filter(({ [direction]: curr }) => {
                    return curr.xml.indexOf(this[direction].xml.split("_C_")[0]) === 0
                })
                .map((el) => el[direction])
        },
        toListNoCity() {
            const direction = 'to';

            console.log('this[direction + "List"]: ', this[direction + "List"]);

            return this[direction + "List"]
                .filter((el, indexId) => {
                    const isCash = el[direction].isCash;

                    if (isCash) {
                        return (indexId === this[direction + "List"].findIndex(item => item[direction].xml.indexOf(el[direction].xml.split("_C_")[0]) === 0))
                    }

                    return true;
                });
        },
        toCities() {
            const direction = 'to';

            if (!this || !this[direction] || !this[direction].isCash) {
                return [];
            }

            return this[direction + "List"]
                .filter(({ [direction]: curr }) => {
                    return curr.xml.indexOf(this[direction].xml.split("_C_")[0]) === 0
                })
                .map((el) => el[direction])
        },

        fromList() {
            let obj = {};
            if (!this.info) return 0; // techworks
            return this.info
                .map(el => {
                    return {
                        ...el,
                        can: this.info.some(m => el.from.xml === m.from.xml && this.to.name === m.to.xml)
                    };
                })
                .filter(el => {
                    if (obj[el.from.xml]) {
                        return false;
                    } else {
                        obj[el.from.xml] = true;
                        return true;
                    }
                })
                .filter(el => !this.symbolFrom || this.symbolFrom === el.from.symbol)
                .sort(function (a, b) {
                    return a.from.positionIn > b.from.positionIn ? 1 : -1;
                });
        },
        toList() {
            let obj = {};
            return this.info
                .filter(el => {
                    if (obj[el.to.xml]) {
                        return false;
                    } else {
                        obj[el.to.xml] = true;
                        return true;
                    }
                })
                .map(el => {
                    return {
                        ...el,
                        can: this.info.some(m => el.to.xml === m.to.xml && this.from.xml === m.from.xml)
                    };
                })
                .filter(el => (!this.symbolTo || this.symbolTo === el.to.symbol) && el.can)
                .sort(function (a, b) {
                    return a.to.positionOut > b.to.positionOut ? 1 : -1;
                });
        },
        fromSymbolList() {
            let obj = {};
            this.info.forEach(el => (obj[el.from.symbol] = true));
            return Object.keys(obj);
        },
        toSymbolList() {
            let obj = {};
            this.info.forEach(el => (obj[el.to.symbol] = true));
            return Object.keys(obj);
        },
        matchedRoute() {
            return (
                this.info.find(el => this.from.xml === el.from.xml && this.to.xml === el.to.xml) || {
                    from: {},
                    rate: {},
                    to: {}
                }
            );
        },
        instructions() {
            if (this.currentRoute && this.currentRoute.instructions) {
                return this.currentRoute.instructions
                    .filter(item => {
                        return item.step === "createOrder";
                    })
                    .map(item => {
                        let resultContent = "";
                        const content = item.content.split("##");
                        for (let i = 0; i < content.length; i++) {
                            if (content[i].length === 2) {
                                if (content[i] !== this.$i18n.locale) {
                                    i++;
                                }
                            } else {
                                resultContent += content[i];
                            }
                        }
                        return { ...item, content: resultContent };
                    });
            } else {
                return [];
            }
        }
    },
    watch: {
        from: {
            deep: true,
            handler() {
                this.changeCurrency();
            }
        },
        to: {
            deep: true,
            handler() {
                this.changeCurrency();
            }
        }
    },
    async created() {
        await this.routes();

        if (process.server) {
            if (this.fromList && this.fromList[0] && this.fromList[0].from)
                this.from = this.fromList[0].from;
            return
        }

        if (!this.info || !this.info[0]) {
            this.errorRoutes = "Routes not found";
        }

        if (!this.info) return 0; // techworks

        let from = String(this.$route.query.from || this.widgetFrom || "");
        let to = String(this.$route.query.to || this.widgetTo || "");
        let amount = this.$route.query.amount || this.widgetAmount;

        if ((!from || !to) && this.fromList && this.toList) {
            this.from = this.fromList[0].from;
            process.nextTick(() => {
                const toCurr = this.toList.find(({ can }) => can);
                if (toCurr) this.to = toCurr.to;
            });
        }

        if (amount) {
            if (amount > 0) {
                this.val1 = Math.abs(amount);
            }
            if (amount < 0) {
                this.val2 = Math.abs(amount);
            }
        }
        let city = String(this.$route.query.city || "");

        let f_f = this.info.find(el => {
            if (el.from.xml === from)
                return true;
            if (city && el.from.xml.indexOf(from + "_C_" + city) === 0)
                return true;
            return false;
        });

        let f_t = this.info.find(el => {
            if (el.to.xml === to)
                return true;
            if (city && el.to.xml.indexOf(to + "_C_" + city) === 0)
                return true;
            return false;
        });

        if (!f_f)
            f_f = this.info.find(el => (el.from.xml.indexOf(to + "_C_") === 0));

        if (!f_t)
            f_t = this.info.find(el => (el.to.xml.indexOf(to + "_C_") === 0));

        if (from && f_f) this.from = { ...f_f.from };
        else this.from = this.fromList[0].from;

        if (to && f_t) this.to = { ...f_t.to };
        else
            process.nextTick(() => {
                const toCurr = this.toList.find(({ can }) => can);
                if (toCurr) this.to = toCurr.to;
            });
    },
    methods: {
        ...mapMutations({
            setLogin: "user/login",
            changeStatusOpenKycModal: "user/changeStatusOpenKycModal",
            setReg: "user/reg",
        }),
        ...mapActions({
            routes: "exchange/routes",
            createOrder: "exchange/createOrder"
        }),
        async checkAndPassVerification(route, byAmountRequired) {
            console.log('profile', this.profile, route);
            if (!route.requiredVerificationUser) return true;
            if (!route.requiredVerificationUser.enable) return true;
            if (route.requiredVerificationUser.minAmount > 0 && !byAmountRequired) return true; // skip for response by api
            if (!this.profile) {
                this.setLogin('startKyc');
                return false;
            }
            if (!this.profile.verificationPerson) {
                this.changeStatusOpenKycModal(true)
                return false;
            }

            return true;
        },
        setVal2() {
            this.val2 = Number(this.currentRoute.rate.amount);
            this.correctFrom();
        },
        setMaxValueTo() {
            this.val2 = Number(this.currentRoute.rate.amount);
            this.correctFrom();
        },
        correctFrom() {
            this.errorFilesMon = false;
            let coef = new BigNumber(this.rateOut).dividedBy(this.rateIn) || 1;
            let num = new BigNumber(this.val2 || 0)
                .plus(this.currentRoute.rate.outFeeAmount)
                .dividedBy(coef)
                .decimalPlaces(this.currentRoute.from.decimal !== undefined ? this.currentRoute.from.decimal : 2);
            if (num.lte(0)) return (this.val1 = 0);

            if (new BigNumber(this.val2).isGreaterThan(this.currentRoute.rate.amount)) {
                this.limitAmount = true;
            } else {
                this.limitAmount = false;
            }
            this.sendAmount = "out";
            this.val1 = num;
            this.minAmount = (num < +this.fixedNumber('ceil', this.currentRoute.from.decimal, this.currentRoute.from.min));
            this.maxAmount = !!Number(this.currentRoute.from.max) && (num > +this.fixedNumber('floor', this.currentRoute.from.decimal, this.currentRoute.from.max));
            process.nextTick(() => {
                let coef1 = new BigNumber(this.rateOut).dividedBy(this.rateIn) || 1;
                if (!coef1.eq(coef)) {
                    this.correctFrom();
                }
            });
        },
        async create() {
            let { routeId } = this.currentRoute;

            if (
                this.currentRoute.from.fields.some(el => el.required && !this.fromValues[el._id]) &&
                this.currentRoute.to.fields.some(el => el.required && !this.toValues[el._id]) &&
                this.currentRoute.fields.some(el => el.required && !this.routeValues[el._id])
            ) {
                return this.$store.dispatch("notify/add", {
                    message: this.$t("page.main.messages.checkFields"),
                    type: "warning"
                });
            }

            if (!this.agree) {
                return this.$store.dispatch("notify/add", {
                    message: this.$t("page.main.messages.mustBeAgree"),
                    type: "warning"
                });
            }


            if (!this.agree_kyc) {
                return this.$store.dispatch("notify/add", {
                    message: this.$t("page.main.messages.mustBeAgree"),
                    type: "warning"
                });
            }

            let fromValues = Object.keys(this.fromValues).map(el => ({
                key: el,
                value: this.fromValues[el]
            }));

            let toValues = Object.keys(this.toValues).map(el => ({
                key: el,
                value: this.toValues[el]
            }));

            let routeValues = Object.keys(this.routeValues).map(el => ({
                key: el,
                value: this.routeValues[el]
            }));

            let order = {
                routeId,
                fromValues,
                toValues,
                routeValues,
                agreement: true,
                captcha: this.captcha,
                email: this.email,
                lang: this.$root.$i18n.locale,
                amount: this.sendAmount === "out" ? -this.val2 : this.val1,
                vuexActionV2: true // vuex param
            };

            const isOkVerification = await this.checkAndPassVerification(this.currentRoute);

            if (!isOkVerification) return;

            let res = await this.createOrder(order).catch(async err => {
                if (5001670807585394 === err.errorCode) {
                    await this.checkAndPassVerification(this.currentRoute, true);

                    return;
                }

                if (1601588666628861 === err.errorCode) {
                    if (this.$refs.recaptcha && this.$refs.recaptcha.reset) this.$refs.recaptcha.reset();
                }

                this.$swal("", err.message, "error");
            });

            if (res) {
                if (!this.profile) {
                    let orders = JSON.parse(localStorage.getItem("orders"));
                    if (orders) orders.push(res);
                    else orders = [res];
                    localStorage.setItem("orders", JSON.stringify(orders));
                }

                this.$router.push(this.localePath(`/order/${res.uid}/${res.secret}/`));
            }
        },

        correctTo() {
            this.errorFilesMon = false;

            if (!this.currentRoute || !this.currentRoute.rate || !this.currentRoute.rate.out || !this.currentRoute.rate.in) {
                this.val2 = 0;
                return null;
            }

            let coef = new BigNumber(this.rateOut).dividedBy(this.rateIn) || 1;
            let num = BigNumber(this.val1 || 0)
                .multipliedBy(coef)
                .minus(this.currentRoute.rate.outFeeAmount)
                .decimalPlaces(this.currentRoute.to.decimal !== undefined ? this.currentRoute.to.decimal : 2);
            if (num.lte(0)) return (this.val2 = 0);

            if (num.isGreaterThan(this.currentRoute.rate.amount)) {
                this.limitAmount = true;
            } else {
                this.limitAmount = false;
            }

            this.sendAmount = "in";
            this.val2 = num;
            this.minAmount = (this.val1 < +this.fixedNumber('ceil', this.currentRoute.from.decimal, this.currentRoute.from.min));
            this.maxAmount = !!Number(this.currentRoute.from.max) && (this.val1 > +this.fixedNumber('floor', this.currentRoute.from.decimal, this.currentRoute.from.max));
        },
        async changeCurrency() {
            let query = {};
            if (this.from.xml) query.from = this.from.xml;
            if (this.to.xml) query.to = this.to.xml;
            if (!this.isWidget && !process.server) this.$router.push({ query });
            if (!this.matchedRoute || !this.matchedRoute.routeId) {
                return false;
            }
            this.loadRouteGetOne = false;
            this.errorloadRouteGetOne = false;
            const res = await this.$rest
                .api("GET:public/exchanger/route/get/one", {
                    id: this.matchedRoute.routeId,
                    lang: this.$i18n.locale
                })
                .catch(err => {
                    this.errorloadRouteGetOne = true;
                    this.loadRouteGetOne = true;
                    return Promise.reject(err);
                });
            if (res.success) this.currentRoute = res.data.route;
            process.nextTick(() => {
                this.loadRouteGetOne = true;
                this.errorloadRouteGetOne = false;
            });
            this.correctTo();
        },
        fixedNumber(type, decimal, number) {
            const divN = Number(10 ** decimal);
            let n = 0;
            if (type === "floor") {
                n = Math.floor(number * divN) / divN;
            } else if (type === "ceil") {
                n = Math.floor(number * divN) / divN;
            } else {
                n = Number(Number(number).toFixed(decimal));
            }
            if (decimal <= 2 && decimal > 0) {
                n = n.toFixed(2);
            } else if (decimal === 0) {
                n = n.toFixed(0);
            }
            return n;
        },

        reverseCurrency() {
            const tempFrom = this.from;
            const tempTo = this.to;

            const found = this.info.find(item => item.from.xml === tempTo.xml && item.to.xml === tempFrom.xml);

            if (found) {
                this.from = tempTo;
                this.to = tempFrom;

                this.$nextTick();
            }
        },

        handleClickFrom(toValue) {
            this.from = toValue;
            this.closeModal();
        },

        handleClickTo(toValue) {
            this.to = toValue;
            this.closeModal();
        },

        closeModal() {
            this.showModalFrom = false;
            this.showModalTo = false;
        },

        openModalFrom() {
            this.showModalFrom = true;
        },

        openModalTo() {
            this.showModalTo = true;
        },

        handleCreate() {
            if (this.formStapTwo) {
                if (this.from && this.to && this.currentRoute.routeId) {
                    this.create();
                }
            }

            if (!this.formStapTwo) {
                if (this.minAmount || this.maxAmount || this.limitAmount || !this.val1 || !this.val2) {
                    this.errorFilesMon = true;
                    return;
                }

                if (this.from && this.to && this.currentRoute.routeId) {
                    this.formStapTwo = true;
                }
            }
        }
    }
};
</script>

<style lang="scss" scoped></style>
<template>
    <Head title="Log in" />

    <jet-authentication-card>
        <jet-validation-errors class="mb-4" />

        <div class="card">
            <div class="card-body login-card-body">
                <p class="login-box-msg">
                    Realize o Login Para Acessar o Sistema
                </p>

                <form @submit.prevent="submit">
                    <div class="input-group mb-3">
                        <jet-input
                            id="email"
                            type="email"
                            class="form-control"
                            v-model="form.email"
                            required
                            autofocus
                        />
                        <div class="input-group-append">
                            <div class="input-group-text">
                                <span class="fas fa-envelope"></span>
                            </div>
                        </div>
                    </div>

                    <div class="input-group mb-3">
                        <jet-input
                            id="password"
                            type="password"
                            class="form-control"
                            v-model="form.password"
                            required
                            autocomplete="current-password"
                        />
                        <div class="input-group-append">
                            <div class="input-group-text">
                                <span class="fas fa-lock"></span>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-8">
                            <div class="input-group mb-3">
                                <label class="flex items-center">
                                    <jet-checkbox
                                        name="remember"
                                        v-model:checked="form.remember"
                                    />
                                    <span class="ml-2 text-sm text-gray-600"
                                        >Remember me</span
                                    >
                                </label>
                            </div>
                        </div>
                        <div class="col-4">
                            <jet-button
                                class="btn btn-primary btn-block"
                                :class="{ 'opacity-25': form.processing }"
                                :disabled="form.processing"
                            >
                                Acessar
                            </jet-button>
                        </div>
                    </div>

                    <div class="input-group mb-3">
                        <Link
                            v-if="canResetPassword"
                            :href="route('password.request')"
                            class="underline text-sm text-gray-600 hover:text-gray-900"
                        >
                            Manter Acesso
                        </Link>
                    </div>
                </form>
            </div>
        </div>
    </jet-authentication-card>
</template>

<script>
import { defineComponent } from "vue";
import JetAuthenticationCard from "@/Jetstream/AuthenticationCard.vue";
import JetAuthenticationCardLogo from "@/Jetstream/AuthenticationCardLogo.vue";
import JetButton from "@/Jetstream/Button.vue";
import JetInput from "@/Jetstream/Input.vue";
import JetCheckbox from "@/Jetstream/Checkbox.vue";
import JetLabel from "@/Jetstream/Label.vue";
import JetValidationErrors from "@/Jetstream/ValidationErrors.vue";
import { Head, Link } from "@inertiajs/inertia-vue3";

export default defineComponent({
    components: {
        Head,
        JetAuthenticationCard,
        JetAuthenticationCardLogo,
        JetButton,
        JetInput,
        JetCheckbox,
        JetLabel,
        JetValidationErrors,
        Link,
    },

    props: {
        canResetPassword: Boolean,
        status: String,
    },

    data() {
        return {
            form: this.$inertia.form({
                email: "",
                password: "",
                remember: false,
            }),
        };
    },

    methods: {
        submit() {
            this.form
                .transform((data) => ({
                    ...data,
                    remember: this.form.remember ? "on" : "",
                }))
                .post(this.route("login"), {
                    onFinish: () => this.form.reset("password"),
                });
        },
    },
});
</script>

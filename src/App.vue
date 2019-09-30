<template>
  <div class="container">
    <h3 class="text-center mt-3">Vuelidate ile Form Kontrolü</h3>
    <div class="d-flex justify-content-center align-content-center flex-row">
      <div class="card p-4 mt-3  shadow">
        <form style="width: 350px" @submit.prevent="onSubmit">

          <div class="form-group">
            <label>E-posta Adresiniz</label>
            <!--@input ile içine yazılanları kontrol edebiliyoruz-->
            <!--@input="$v.email.$touch()"-->
            <input
            @blur="$v.email.$touch()"
            :class="{'is-invalid': $v.email.$error}"
            v-model="email" 
            type="email" class="form-control" 
            placeholder="E-posta adresini giriniz">
            <small v-if="!$v.email.required" class="form-text text-danger">*Buranın doldurulması zorunludur</small>
            <small v-if="!$v.email.email" class="form-text text-danger">*Eksik veya yanlış e-posta girdiniz..</small>
          </div>

          <div class="form-group">
              <label>Şifre</label>
              <input v-model="$v.password.$model" type="password" class="form-control"
                     placeholder="Şifrenizi giriniz">
              <small v-if="!$v.password.numeric" class="form-text text-danger">Lütfen şifreniz rakamlardan
                  oluşsun...
              </small>
              <small v-if="!$v.password.required" class="form-text text-danger">Bu alan zorunludur...</small>
              <small v-if="!$v.password.minLength" class="form-text text-danger">Lütfen şifreniz en az 
              {{$v.password.$params.minLength.min }} karakterden oluşmalıdır...
              </small>
              <small v-if="!$v.password.maxLength" class="form-text text-danger">Lütfen şifreniz en fazla 
              {{ $v.password.$params.maxLength.max }} karakterden oluşmalıdır...
              </small>
          </div>

          <div class="form-group">
              <label>Şifre Tekrar</label>
              <input v-model="$v.repassword.$model" type="password" class="form-control"
                     placeholder="Şifrenizi tekrar giriniz">
              <small v-if="!$v.repassword.required" class="form-text text-danger">Bu alan zorunludur...
              </small>
              <small v-if="!$v.repassword.numeric" class="form-text text-danger">Lütfen şifreniz rakamlardan
                  oluşsun...
              </small>
              <small v-if="!$v.repassword.minLength" class="form-text text-danger">Lütfen şifreniz en az 
              {{$v.repassword.$params.minLength.min }} karakterden oluşmalıdır...
              </small>
              <small v-if="!$v.repassword.maxLength" class="form-text text-danger">Lütfen şifreniz en fazla 
              {{ $v.repassword.$params.maxLength.max }} karakterden oluşmalıdır...
              </small>
              <small v-if="!$v.repassword.sameAs" class="form-text text-danger">Girdiğiniz şifreler
                  birbirleriyle uyuşmuyor...
              </small>
          </div>

          <div class="form-group">
              <label>Yaşınız</label>
              <input v-model="$v.age.$model" type="text" class="form-control"
                     placeholder="Yaşınızı giriniz">
              <small v-if="!$v.age.required" class="form-text text-danger">Bu alan zorunludur...</small>
               <small v-if="!$v.age.numeric" class="form-text text-danger">Lütfen sadece rakam giriniz...
              </small>
              <small v-if="!$v.age.between" class="form-text text-danger">
                  Kayıt olabilmeniz için yaşınızın {{ $v.age.$params.between.min }} ile 
                  {{$v.age.$params.between.max }} olması gerekir..
              </small>
          </div>
          <button class="btn btn-outline-secondary rounded-0">Kaydet</button>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
//Hangi validationsu kullanmak istiyorsak onu süslü parantez içine yazmamız gerekir
import {required, email, numeric, minLength, maxLength, sameAs,between} from "vuelidate/lib/validators"
  export default {
    //Öncellikle validationsu nereye uygulayacaksak onu data() içinde tanımlamamız gerekiyor.
    data() {
      return {
        email : null,
        password : null,
        repassword : null,
        age: null
       
      }
    },
      validations : {
        email: {
            required, //ES6 dan dolayı böyle tenımlama yapabiliriz.(reqiured : required)
            email
        },
        password: {
                required,
                numeric,
                minLength: minLength(6),
                maxLength: maxLength(8)
            },
            repassword: {
                required,
                numeric,
                minLength: minLength(6),
                maxLength: maxLength(8),
                // sameAs : sameAs('password'),
                //sameAs'in farklı kullanımı
                sameAs: sameAs(vm => {//şunun gibi anlamına gelir --> şununla aynıysa
                    return vm.password + "78"
                })
            },
            age: {
                required,
                numeric,
                between: between(18, 60)
            }
    },
    methods: {
      onSubmit(){
        let form = {
          email : this.email,
          password : this.password,
          age: this.age
        }
        console.log(form)
      }
      
    }
  }
</script>

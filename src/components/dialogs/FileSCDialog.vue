<template>
    <v-card class="secondary--text">
        <v-btn class="mt-4 mb-0" color="accent" text>
            <v-icon>mdi-chevron-left</v-icon>
            view case
        </v-btn>
        <!-- <v-card-title class="my-2 display-1 secondary--text font-weight-medium">Summons and Complaints</v-card-title> -->
        <!-- <v-card-subtitle v-if="subtitle"><span class="font-weight-medium">Note:</span> {{subtitle}} </v-card-subtitle> -->
  <v-stepper non-linear alt-labels v-model="e1">
    <v-stepper-header>
      <v-stepper-step :complete="e1> 1" step="1" editable edit-icon="mdi-check">Review</v-stepper-step>

      <v-divider></v-divider>

      <v-stepper-step :complete="e1 > 2" step="2" editable edit-icon="mdi-check">Accept</v-stepper-step>

      <v-divider></v-divider>

      <v-stepper-step step="3" editable edit-icon="mdi-check">File</v-stepper-step>
    </v-stepper-header>

    <v-stepper-items>
      <v-stepper-content step="1">
          <v-card-title class="my-2 display-1 secondary--text font-weight-medium">Review Adjournment Request</v-card-title>
        
            <pdf-viewer />
      <div class="d-flex justify-end">
        <v-btn rounded color="accent" dark class="px-8" small depressed
          @click="e1 = 2"
        >
          Continue
        </v-btn>
        <v-btn
                color="accent"
                text
                @click="()=>this.$emit('change:dialog', '')"
            >
                cancel
            </v-btn>
      </div>
      </v-stepper-content>

      <v-stepper-content step="2">
          <div class="secondary--text font-weight-medium my-2">
            When you click “Accept,” <span class="font-weight-bold"> {{dialogCase.firstname}} {{dialogCase.lastname}}</span> will receive the automated email you have previously created, tailored for this case.
            You may use the space below to ask questions and/or provide information pertaining to this case.
        </div>
        <div class="my-4 secondary--text">
            <v-icon color="primary" small>mdi-alert-circle</v-icon>
            Haven’t set up your email templates? You may create them <router-link :to="{ name: 'main' }">here</router-link></div>

        <div class="custom-overline  info--text font-weight-medium  mb-1"> Message </div>
        <v-textarea
          class="mb-4"
          filled
          no-resize
          background-color="#F0F5F6"
          v-model="email"
        ></v-textarea>

        <div class="d-flex justify-end">
        <v-btn rounded color="accent" dark class="px-8" small depressed
          @click="e1 = 3"
        >
          Continue
        </v-btn>
        <v-btn
                color="accent"
                text
                @click="()=>this.$emit('change:dialog', '')"
            >
                cancel
            </v-btn>
        </div>

      </v-stepper-content>

      <v-stepper-content step="3">
        <div class="d-flex justify-end">
        <v-btn rounded color="accent" dark class="px-8" small depressed @click="submit">File</v-btn>
        </div>
<!-- 
        <v-btn
          color="primary"
          @click="e1 = 1"
        >
          Continue
        </v-btn>

        <v-btn text>Cancel</v-btn> -->
      </v-stepper-content>
    </v-stepper-items>
  </v-stepper>
            <!-- <v-card-actions>
            <v-spacer></v-spacer>

            <v-btn
                color="accent"
                text
                @click="()=>this.$emit('change:dialog', '')"
            >
                cancel
            </v-btn>

            <v-btn rounded color="accent" dark class="px-8" small depressed @click="submit">File</v-btn>
            </v-card-actions> -->
    </v-card>

</template>

<script>
import axios from 'axios'
import PdfViewer from '@/components/PdfViewer'
export default {
    name: 'fileSCDialog',
    data() {
        return{
            e1: 1,
            email: `Dear ${this.dialogCase.firstname},
Thank you for choosing me to review your Notice to Cease. Please allow 24 hours for review your documentation and the pleading. I will…`
        }
    },
    components: {
        PdfViewer,
    },
    props: {
        dialogCase: Object,
        dialogAction: Object,
    },
    methods:{
        submit(){
            axios
                .post(`http://localhost:3333/fileSC`,{
                        caseId: this.dialogAction.CaseId,
                        caseActionId: this.dialogAction.CaseActionId
                        })
                        .then((response) => {
                        console.log(response);
                        this.$store.dispatch('loadCases')
                        this.$emit('change:dialog', '')
                        }, (error) => {
                        console.log(error);
                        })
        },
    },

}
</script>

<style>

</style>
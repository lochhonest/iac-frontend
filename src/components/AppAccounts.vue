<template>
    <div class="container">
        <div class="mb-4">
            <h1>Recipes</h1>
            <hr />
            <!-- Alert Message -->
            <b-alert v-if="showMessage" variant="success" show
                >{{ message }}</b-alert
            >
            <!-- b-alert v-if="error" variant="danger" show>{{ error }}</b-alert-->

            <button
                type="button"
                class="btn btn-success btn-sm"
                v-b-modal.recipe-modal
            >
                Create
            </button>
            <br />
            <br />
            <table class="table table-hover">
                <thead>
                    <tr>
                        <th scope="col">Recipe Name</th>
                        <th scope="col">Recipe Ingredients</th>
                        <th scope="col">Recipe steps</th>
                        <th scope="col">Recipe Favorite</th>
                        <th scope="col">Recipe Rating</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="recipe in recipes" :key="recipe.id">
                        <td>{{ recipe.name }}</td>
                        <td>{{ recipe.ingredients }}</td>
                        <td>{{ recipe.steps }}</td>
                        <td>
                            <b-icon
                                v-if="recipe.favorite"
                                icon="heart-fill"
                            ></b-icon>
                            <b-icon
                                v-if="!recipe.favorite"
                                icon="heart"
                            ></b-icon>
                        </td>
                        <td>
                            <div>
                                <b-form-rating
                                    v-model="recipe.rating"
                                    readonly
                                    variant="warning"
                                    class="mb-2"
                                ></b-form-rating>
                                <p class="mt-2"></p>
                            </div>
                        </td>
                        <td>
                            <div class="btn-group" role="group">
                                <button
                                    type="button"
                                    class="btn btn-info btn-sm"
                                    v-b-modal.edit-recipe-modal
                                    @click="editRecipe(recipe)"
                                >
                                    Edit
                                </button>
                                <button
                                    type="button"
                                    class="btn btn-danger btn-sm"
                                    @click="deleteRecipe(recipe)"
                                >
                                    Delete
                                </button>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <footer class="text-center"></footer>

        <!-- Beginning of Modal for Create -->
        <div>
            <b-modal
                ref="addRecipeModal"
                id="recipe-modal"
                title="Add Recipe"
                hide-backdrop
                hide-footer
            >
                <b-form @submit="onSubmit" class="w-100">
                    <b-form-group
                        id="name"
                        label="Recipe Name:"
                        label-for="name"
                        description="Name of the recipe."
                    >
                        <b-form-input
                            v-model="recipeForm.name"
                            id="name"
                            type="text"
                            required
                        ></b-form-input>
                    </b-form-group>
                    <b-form-group
                        id="ingredients"
                        label="Ingredients:"
                        label-for="ingredients"
                        description="List of ingredients needed for the recipe, separated by commas."
                    >
                        <b-form-input
                            v-model="recipeForm.ingredients"
                            id="ingredients"
                            type="text"
                            required
                        ></b-form-input>
                    </b-form-group>

                    <b-form-group
                        id="steps"
                        label="Steps:"
                        label-for="steps"
                        description="Instructions for preparing the recipe."
                    >
                        <b-form-textarea
                            v-model="recipeForm.steps"
                            id="steps"
                            rows="5"
                            required
                        ></b-form-textarea>
                    </b-form-group>

                    <b-form-group
                        id="favorite"
                        label="Favorite:"
                        label-for="favorite"
                        description="Mark as a favorite recipe."
                    >
                    <input v-model="recipeForm.favorite" type="checkbox" id="favorite" >
                    </b-form-group>

                    <b-form-group
                        id="rating"
                        label="Rating:"
                        label-for="rating"
                        description="Rate the recipe."
                    >
                        <b-form-rating
                            v-model="recipeForm.rating"
                            id="rating"
                        ></b-form-rating>
                    </b-form-group>

                    <b-button type="submit" variant="primary" class="mr-2">
                        Save
                    </b-button>
                    <b-button
                        type="reset"
                        variant="secondary"
                        @click="$refs.addRecipeModal.hide()"
                    >
                        Cancel
                    </b-button>
                </b-form>
            </b-modal>
        </div>

        <!-- Beginning of Modal for Edit -->
        <div>
            <b-modal
                ref="editRecipeModal"
                id="edit-recipe-modal"
                title="Edit Recipe"
                hide-backdrop
                hide-footer
            >
                <b-form @submit="onSubmitUpdate" class="w-100">
                    <b-form-group
                        id="edit-name"
                        label="Recipe Name:"
                        label-for="edit-name"
                        description="Name of the recipe."
                    >
                        <b-form-input
                            v-model="editForm.name"
                            id="edit-name"
                            type="text"
                            required
                        ></b-form-input>
                    </b-form-group>
                    <b-form-group
                        id="edit-ingredients"
                        label="Ingredients:"
                        label-for="edit-ingredients"
                        description="List of ingredients needed for the recipe, separated by commas."
                    >
                        <b-form-input
                            v-model="editForm.ingredients"
                            id="edit-ingredients"
                            type="text"
                            required
                        ></b-form-input>
                    </b-form-group>

                    <b-form-group
                        id="edit-steps"
                        label="Steps:"
                        label-for="edit-steps"
                        description="Instructions for preparing the recipe."
                    >
                        <b-form-textarea
                            v-model="editForm.steps"
                            id="edit-steps"
                            rows="5"
                            required
                        ></b-form-textarea>
                    </b-form-group>

                    <b-form-group
                        id="edit-favorite"
                        label="Favorite:"
                        label-for="edit-favorite"
                        description="Mark as a favorite recipe."
                    >
                    <input v-model="editForm.favorite" type="checkbox" id="favorite" >
                    </b-form-group>

                    <b-form-group
                        id="edit-rating"
                        label="Rating:"
                        label-for="edit-rating"
                        description="Rate the recipe."
                    >
                        <b-form-rating
                            v-model="editForm.rating"
                            id="edit-rating"
                        ></b-form-rating>
                    </b-form-group>

                    <b-button type="submit" variant="primary" class="mr-2">
                        Save
                    </b-button>
                    <b-button
                        type="reset"
                        variant="secondary"
                        @click="$refs.editRecipeModal.hide()"
                    >
                        Cancel
                    </b-button>
                </b-form>
            </b-modal>
        </div>
    </div>
</template>

<script>


    import axios from 'axios';
    export default
    {
      name: 'AppRecipes',
      data()
      {
          return {
              recipes: [],
              recipeForm: {
                  name: '',
                  ingredients: '',
                  steps: '',
                  favorite: false,
                  rating: 0
              },

              editForm: {
                  name: '',
                  ingredients: '',
                  steps: '',
                  favorite: false,
                  rating: 0
              },

              showMessage: false,
              message: '',

          };
      },
      methods: {

        onFavoriteChange() {
    console.log(this.recipeForm.favorite);
},

          //GET Recipes
          RESTgetRecipes() {
          const path = `${process.env.VUE_APP_ROOT_URL}/recipes`;
          axios
              .get(path)
              .then((response) => {
                  this.recipes = response.data.recipe;

                      // To actually show the message
                      this.showMessage = true;
                      // To hide the message after 5 seconds
                      setTimeout(() => {
                        this.showMessage = false;
                      }, 100000);
              }
              )
              .catch((error) => {
                  console.log(error);
              });
          },

          //POST Recipes
          RESTcreateRecipe(payload){
              const path = `${process.env.VUE_APP_ROOT_URL}/recipes`;
              axios
                  .post(path,payload)
                  .then((response) => {
                      this.RESTgetRecipes();
                      this.message = "Recipe Created succesfully!";
                      // To actually show the message
                      this.showMessage = true;
                      // To hide the message after 5 seconds
                      setTimeout(() => {
                        this.showMessage = false;
                      }, 50000);
                  })
                  .catch((error) => {
                      console.log(error);
                      this.RESTgetRecipes()
                  });
          },

          RESTupdateRecipe(payload, id){
              const path = `${process.env.VUE_APP_ROOT_URL}/recipe/${id}`;
              axios
                  .put(path,payload)
                  .then((response) => {
                      this.RESTgetRecipes();

                        this.message = "Recipe Updated Successfully"
                        this.showMessage = true;
                        setTimeout(() => {
                            this.showMessage = false;
                        }, 3000);

                  })
                  .catch((error) => {
                      console.log(error);
                      this.RESTgetRecipes()
                  });
          },


          RESTdeleteRecipe(id){
              const path = `${process.env.VUE_APP_ROOT_URL}/recipes/${id}`;
              axios
                  .delete(path)
                  .then((response) => {
                      this.RESTgetRecipes();
                      this.message = "Recipe Deleted Successfully"
                      this.showMessage = true;
                      set.TimeOut(() => {
                          this.showMessage = false;
                      }, 3000);

                  })
                  .catch((error) => {
                      console.log(error);
                      this.RESTgetRecipes()
                  });
          },

          initForm(){
              this.recipeForm = {
                  name: '',
                  ingredients: '',
                  steps: '',
                  favorite: false,
                  rating: 0
              };
              this.editForm = {
                  name: '',
                  ingredients: '',
                  steps: '',
                  favorite: false,
                  rating: 0
              };

          },
          onSubmit(e) {
              e.preventDefault();
              this.$refs.addRecipeModal.hide();
              const payload = {
                  name: this.recipeForm.name,
                  ingredients: this.recipeForm.ingredients,
                  steps: this.recipeForm.steps,
                  favorite: this.recipeForm.favorite,
                  rating: this.recipeForm.rating
              };
              this.RESTcreateRecipe(payload);
              this.initForm();
          },
          onSubmitUpdate(e) {
            e.preventDefault();
            this.$refs.editRecipeModal.hide();
            const payload = {
                name: this.editForm.name,
                ingredients: this.editForm.ingredients,
                steps: this.editForm.steps,
                favorite: this.editForm.favorite,
                rating: this.editForm.rating
            };
            this.RESTupdateRecipe(payload, this.editForm.id);
            this.initForm();
        },

          deleteRecipe(recipe) {
              this.RESTdeleteRecipe(recipe.id);
          },

          editRecipe(recipe) {
              this.editForm = recipe;
          },



      },


      created() {
          this.RESTgetRecipes();
      },
    };
</script>
<style>
    .vertical-center {
        min-height: 100%; /* Fallback for browsers do NOT support vh unit */
        min-height: 100vh; /* These two lines are counted as one :-)       */
        display: flex;
        align-items: center;
    }
</style>

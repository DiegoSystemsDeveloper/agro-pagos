<template>
  <b-container class="p-5">
    <b-card no-body class="overflow-hidden">
      <b-row>
        <b-col md="6">
          <b-card-body title="Iniciar sesión">
            <b-form>
              <b-form-group
                id="input-group-email"
                label="Dirección de correo electrónico:"
                label-for="input-email"
              >
                <b-form-input
                  id="input-email"
                  type="email"
                  required
                  placeholder="Ingrese correo electrónico"
                ></b-form-input>
              </b-form-group>

              <b-form-group
                id="input-group-password"
                label="Contraseña:"
                label-for="input-password"
              >
                <b-form-input
                  id="input-password"
                  type="password"
                  required
                  placeholder="Ingrese su contraseña"
                ></b-form-input>
              </b-form-group>

              <b-button block class="mt-4" variant="primary" type="submit"
                >Ingresar</b-button
              >
            </b-form>
            <div class="text-center mt-2">
              <p>
                <a href="#" class="mt-2">¿Olvidaste tu contraseña?</a>
              </p>
            </div>
          </b-card-body>
        </b-col>
        <b-col md="6">
          <img
            src="https://cdn.pixabay.com/photo/2014/09/19/19/09/peasant-452904__340.jpg"
            alt="Image"
            class="rounded-0"
          />
        </b-col>
      </b-row>
    </b-card>
  </b-container>
</template>

<script>
export default {
  name: 'Login',  
}
</script>
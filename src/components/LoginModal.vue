<template>
    <div class="circular">
        <span class="close" @click="$emit('myEvent')">&#10060;</span>
        <h1>Login</h1>
        <form class="d-flex flex-column justify-content-around m-3">
            <label  for="email">Email:</label>
            <input required type="email" id="email" name="email" class="form-control">
            <label required for="password">Password:</label>
            <input type="password" id="password" name="password" class="form-control">
            <button type="submit" class="btn btn-secondary mt-3">Login</button>
        </form>
    </div>
</template>
<script setup>

</script>
<style scoped>
form button {
    background-color: var(--blue-primary);
    border-color: var(--blue-primary);
    color: white;
    padding: 0.5rem;
    border-radius: 0.3rem;
    cursor: pointer;
    width: 100%;
}

form button:hover {
    transform: scale(1.1);
    background-color :var(--second-color)
}
.form-control {
    margin-bottom: 1rem; 
    padding: 0.5rem; 
    border: 1px solid #ccc; 
    border-radius: 0.3rem; 
    width: 100%; 
    
}

.circular{
    width: 18rem;
    display: flex;
    flex-direction: column;
    -webkit-box-shadow: 6px 1px 87px 37px var(--blue-primary);
    -moz-box-shadow: 6px 1px 87px 37px var(--blue-primary);
    box-shadow: 6px 1px 87px 37px var(--blue-primary);
    border-radius: 1rem 1rem 1rem 1rem;
    background-color: white;
    position: relative;

}

.close {
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
}
</style>
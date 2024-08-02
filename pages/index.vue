<script setup>
</script>

<template>
    <div class="grid">
        <div class="col-6">
            <RouterLink to="/barzellette" class="card-link">
                <Card>
                    <template #header>
                        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQQMpTPsQbjax6Zre04ih6wrRiGKyJpqgYIRA&s" alt="Barzellette" class="card-image"/>
                    </template>
                </Card>
            </RouterLink>
        </div>
        <div class="col-6">
            <RouterLink to="/frasi" class="card-link">
                <Card>
                    <template #header>
                        <img src="https://www.lafeltrinelli.it/images/9788834124154_0_536_0_75.jpg" alt="Frasi" class="card-image"/>
                    </template>
                </Card>
            </RouterLink>
        </div>
    </div>
</template>

<style scoped>
.card {
    max-width: 31rem; 
}

.card-image {
    width: 100%; 
}
</style>



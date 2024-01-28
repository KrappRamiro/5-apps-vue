<!-- InputTag.vue -->
<script>
export default {
    emits: ["onTagsChange"],
    data() {
        return {
            currentValue: "",
            tags: []
        }
    },
    methods: {
        handleSubmit() {
            if (this.currentValue === "") {
                return
            }
            if (this.tags.includes(this.currentValue)) {
                return
            }
            this.tags.push(this.currentValue)
            this.currentValue = ""
            this.$emit("onTagsChange", this.tags)

        },

        deleteTag(tag) {
            this.tags = this.tags.filter(item => item !== tag)
            this.$emit("onTagsChange", this.tags)

        },

        handleKeyDown(e) {
            if (e.key == "Backspace" && this.currentValue === "") {
                this.tags.pop();
                this.$emit("onTagsChange", this.tags)
            }
        }
    }
}
</script>

<template>
    <div class="inputTag">
        <div class="tags">
            <div class="tag" v-for="(tag, index) in tags" :key="index">
                {{ tag }} <button @click="deleteTag(tag)" class="delete-button">X</button>
            </div>
        </div>
        <form @submit.prevent="handleSubmit" class="form">
            <input type="text" class="input" v-model="currentValue" @keydown="handleKeyDown">
        </form>
    </div>
</template>

<style scoped>
.inputTag {
    display: inline-flex;
    border: solid 1px #000;
    border-radius: 3px;
    height: 45px;
}

.tags {
    display: flex;
    gap: 3px;
    padding: 5px;
}

.tag {
    display: flex;
    padding: 5px;
    border: solid 1px #ccc;
    gap: 5px;
    align-content: 5px;
    border-radius: 3px;
}

.input {
    border: none;
    outline: none;
    padding: 0 5px;
}

.form {
    display: inline-flex;
}

.delete-button {
    background-color: transparent;
    border: none;
    border-radius: 3px;
    cursor: pointer;
}

.delete-button:hover {
    background-color: #eee;
}
</style>

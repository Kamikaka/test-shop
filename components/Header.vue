<template>
    <header>
        <div class="container">
            <div class="bg-box header_padding-bg-box">
                <div class="row jc_space-between ai_center">
                    <div class="header_left-icons">
                        <div class="header-icons row ai_center">
                            <a href="#" class="header-icon mail">
                                <svg-icon name="mail" />
                            </a>
                            <a href="#" class="header-icon calendar">
                                <svg-icon name="calendar" />
                            </a>
                            <a href="#" class="header-icon star">
                                <svg-icon name="star" />
                            </a>
                        </div>
                    </div>
                    <div class="header_right-icons row">
                        <div class="header-icons row ai_center">
                            <a href="#" class="header-icon bell">
                                <svg-icon name="bell" />
                            </a>
                            <a href="#" class="header-icon search">
                                <svg-icon name="search" />
                            </a>
                        </div>
                        <div class="header-user-info row" v-if="user">
                            <div class="header-user-info-name">
                                <div class="user-name">{{ user.name }}</div>
                                <div class="user-status">{{ user.status }}</div>
                            </div>
                            <div class="user-avatar">
                                <img :src=user.avatar :alt=user.name>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>
<script>
    export default {
        data: () => ({
            user: {
                name: "John Doe",
                status: "Available",
                avatar: "/img/user.png"
            },
        }),
    }
</script>
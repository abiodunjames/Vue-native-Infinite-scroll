<template>
    <nb-container>
        <nb-header>
            <nb-body>
                <nb-title>Infinite Scroll</nb-title>
            </nb-body>
        </nb-header>
        <scroll-view :on-scroll="(event) => {loadMore(event)}" :scroll-event-throttle="400">
            <nb-content>
                <nb-list-item avatar v-for="user in users">
                    <nb-left>
                        <nb-thumbnail small :source="{uri: user.avatar}"/>
                    </nb-left>
                    <nb-body>
                        <nb-text>{{user.first_name}} {{user.last_name }}</nb-text>
                    </nb-body>
                </nb-list-item>
            </nb-content>
        </scroll-view>
        <view :style="{flex: 1, justifyContent: 'center'}" v-if="loading">
            <activity-indicator size="large" color="gray"/>
        </view>
    </nb-container>

</template>

<script>
    import axios from "axios"

    export default {
        data: function () {
            return {
                loading: false,
                users: [],
                per_page: 15
            }
        },
        mounted: function () {
            this.getData();
        },

        methods: {
            getData: function () {
                let uri = 'https://reqres.in/api/users';
                this.loading = true;
                axios.get(uri, {
                    params: {
                        per_page: this.per_page
                    }
                }).then((result) => {
                    let response = result.data.data;
                    console.log(response);
                    for (let i in response) {
                        this.users.push(response[i]);
                    }
                    this.loading = false;
                }).catch((error) => {
                    console.log(error)
                })
            },
            loadMore: function (event) {
                let paddingToBottom = 0;
                paddingToBottom += event.nativeEvent.layoutMeasurement.height;
                if (event.nativeEvent.contentOffset.y >= event.nativeEvent.contentSize.height - paddingToBottom) {
                    this.getData()
                }
            }
        }
    }
</script>

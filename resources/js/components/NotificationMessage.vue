<template>
    <div>
        <div
            class="notification table table-fixed w-full"
            :class="{ 'cursor-pointer': this.notification.data.url }"
            @click="handleClick"
        >
            <span class="table-cell w-8 align-top py-4">
                <span v-if="notification.data.level === 'success'">
                    <svg
                        aria-hidden="true"
                        data-prefix="fas"
                        data-icon="check-circle"
                        class="svg-inline--fa fa-check-circle fa-w-16"
                        role="img"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 512 512"
                    >
                        <path
                            fill="#88bb71"
                            d="M504 256c0 136.967-111.033 248-248 248S8 392.967 8 256 119.033 8 256 8s248 111.033 248 248zM227.314 387.314l184-184c6.248-6.248 6.248-16.379 0-22.627l-22.627-22.627c-6.248-6.249-16.379-6.249-22.628 0L216 308.118l-70.059-70.059c-6.248-6.248-16.379-6.248-22.628 0l-22.627 22.627c-6.248 6.248-6.248 16.379 0 22.627l104 104c6.249 6.249 16.379 6.249 22.628.001z"
                        ></path>
                    </svg>
                </span>
                <span v-if="notification.data.level === 'info'">
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
                        />
                    </svg>
                </span>
                <span v-else-if="notification.data.level === 'warning'">
                    <svg
                        aria-hidden="true"
                        data-prefix="fas"
                        data-icon="exclamation-triangle"
                        class="svg-inline--fa fa-exclamation-triangle fa-w-18"
                        role="img"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 576 512"
                    >
                        <path
                            fill="#f4c739"
                            d="M569.517 440.013C587.975 472.007 564.806 512 527.94 512H48.054c-36.937 0-59.999-40.055-41.577-71.987L246.423 23.985c18.467-32.009 64.72-31.951 83.154 0l239.94 416.028zM288 354c-25.405 0-46 20.595-46 46s20.595 46 46 46 46-20.595 46-46-20.595-46-46-46zm-43.673-165.346l7.418 136c.347 6.364 5.609 11.346 11.982 11.346h48.546c6.373 0 11.635-4.982 11.982-11.346l7.418-136c.375-6.874-5.098-12.654-11.982-12.654h-63.383c-6.884 0-12.356 5.78-11.981 12.654z"
                        ></path>
                    </svg>
                </span>
                <span v-else-if="notification.data.level === 'error'">
                    <svg
                        aria-hidden="true"
                        data-prefix="fas"
                        data-icon="exclamation-circle"
                        class="svg-inline--fa fa-exclamation-circle fa-w-16"
                        role="img"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 512 512"
                    >
                        <path
                            fill="#c62828"
                            d="M504 256c0 136.997-111.043 248-248 248S8 392.997 8 256C8 119.083 119.043 8 256 8s248 111.083 248 248zm-248 50c-25.405 0-46 20.595-46 46s20.595 46 46 46 46-20.595 46-46-20.595-46-46-46zm-43.673-165.346l7.418 136c.347 6.364 5.609 11.346 11.982 11.346h48.546c6.373 0 11.635-4.982 11.982-11.346l7.418-136c.375-6.874-5.098-12.654-11.982-12.654h-63.383c-6.884 0-12.356 5.78-11.981 12.654z"
                        ></path>
                    </svg>
                </span>
            </span>
            <div class="table-cell w-full py-4 pl-4">
                <p
                    style="color: black;
"
                >
                    {{ notification.data.message }}
                </p>
                <span class="text-sm text-70">{{ notification.created_at | fromNow }}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'NotificationMessage',
    props: ['notification'],
    filters: {
        fromNow(date) {
            return new moment.tz(date.date, 'YYYY-MM-DD HH:mm:ss', date.timezone).local().fromNow();
        },
    },
    methods: {
        handleClick() {
            if (this.notification.data.url) {
                let win = window.open(
                    this.notification.data.url,
                    this.notification.data.target || '_blank'
                );

                if (win) {
                    win.focus();
                }
            }
        },
    },
    created() {
        // Set interval to force update every minute
        this.interval = setInterval(() => this.$forceUpdate(), 60000);
    },
    beforeDestroy() {
        clearInterval(this.interval);
    },
};
</script>

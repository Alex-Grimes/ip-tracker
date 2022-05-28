<template>
    <div
    class="absolute
    -bottom-64
    lg:-bottom-20
    flex
    flex-col
    lg:flex-row
    bg-white
    gap-y-4
    gap-x-12
    px-8 pt-8
    pb-12
    rounded-md" >
        <!-- Flex item one -->
        <div class="flex flex-col">
            <h3 class="text-xs mb-2 uppercase">IP Address</h3>
            <span class="text-2xl">{{ipInfo.address}}</span>
        </div>
                <!-- Flex item two -->
        <div class="flex flex-col">
            <h3 class="text-xs mb-2 uppercase">Location</h3>
            <span class="text-2xl">{{ipInfo.state}}</span>
        </div>
                <!-- Flex item three -->
        <div class="flex flex-col">
            <h3 class="text-xs mb-2 uppercase">Timezone</h3>
            <span class="text-2xl">{{ipInfo.timezone}}</span>
        </div>
                <!-- Flex item four -->
        <div class="flex flex-col">
            <h3 class="text-xs mb-2 uppercase">Isp</h3>
            <span class="text-2xl capitalize">{{ipInfo.isp}}</span>
        </div>
    </div>
</template>

<script>
export default {
  props: ['ipInfo']
}
</script>

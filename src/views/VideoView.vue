<template>
  <q-page v-scroll="scrolled">
    <q-card flat class="open-gpu">
      <q-card-section class="no-padding" :horizontal="Screen.gt.sm">
        <!-- 视频 -->
        <q-card-section
          v-intersection="commentOnIn"
          :class="
            Screen.gt.sm
              ? 'col-6 overflow-hidden window-height  '
              : 'col-12 no-padding'
          "
        >
          <q-responsive
            class="q-mx-auto"
            :ratio="16 / 9"
            style="max-height: 600px; max-width: 800px"
          >
            <div id="player"></div>
            <q-resize-observer :debounce="500" @resize="onResize" />
          </q-responsive>

          <!-- 视频信息 -->
          <div
            :style="{ height: Screen.gt.sm ? videoInfoHeight.h : 'auto' }"
            :class="Screen.gt.sm ? 'overflow-auto' : ''"
          >
            <!-- 发布者 -->
            <q-toolbar class="q-py-sm">
              <q-avatar class="cursor-pointer" size="md">
                <q-img src="/avatar.jpeg">
                  <template v-slot:loading>
                    <q-spinner-orbit color="primary" size="sm" />
                  </template>
                </q-img>
                <!-- 用户认证 -->
                <q-icon class="absolute-bottom-right ren-zheng">
                  <svg
                    t="1709780406929"
                    class="icon"
                    viewBox="0 0 1024 1024"
                    version="1.1"
                    xmlns="http://www.w3.org/2000/svg"
                    p-id="4742"
                    width="200"
                    height="200"
                  >
                    <path
                      d="M855.458909 138.379636a622.312727 622.312727 0 0 1-197.818182-48.546909c-37.608727-16.989091-95.883636-55.854545-129.861818-80.058182a51.153455 51.153455 0 0 0-58.274909 0c-33.978182 24.203636-92.206545 63.069091-131.072 80.058182a622.312727 622.312727 0 0 1-197.818182 48.546909C113.943273 139.589818 93.277091 162.629818 93.277091 189.346909v305.803636a456.657455 456.657455 0 0 0 214.807273 387.165091l94.673454 59.485091 67.956364 42.496a52.596364 52.596364 0 0 0 53.434182 0l67.956363-42.496 94.673455-59.485091a456.657455 456.657455 0 0 0 214.807273-387.118545V188.136727a47.662545 47.662545 0 0 0-46.126546-49.757091z"
                      fill="#45BE89"
                      p-id="4743"
                    ></path>
                    <path
                      d="M498.641455 638.370909a34.909091 34.909091 0 0 1-25.460364 10.938182h-3.677091a38.306909 38.306909 0 0 1-26.670545-15.778909l-134.749091-133.492364c-10.891636-12.101818-15.732364-36.398545 1.256727-53.387636 15.778909-14.568727 40.029091-12.148364 58.228364 6.050909l111.662545 103.144727 200.238545-200.238545a37.794909 37.794909 0 0 1 52.177455 1.210182c10.938182 14.568727 10.938182 37.655273-2.420364 50.967272l-230.586181 230.586182z"
                      fill="#FFFFFF"
                      p-id="4744"
                    ></path>
                  </svg>
                </q-icon>
              </q-avatar>
              <q-toolbar-title class="text-body1 cursor-pointer flex column">
                <div class="ellipsis full-width">
                  昵称昵称昵称昵称昵称昵称昵称昵称昵称
                </div>
                <div class="flex q-gutter-x-sm items-center">
                  <q-badge color="green" text-color="white" label="审核员" />

                  <!-- ip -->
                  <div class="text-caption flex items-center">
                    <q-icon name="place" />贵州
                  </div>

                  <div class="text-caption flex items-center">
                    <!-- female男 女male -->
                    <q-icon name="male" color="pink" />
                  </div>
                </div>
              </q-toolbar-title>

              <!-- 0 未关注 add  |1 已关注 check | 2 已互关 done_all  -->
              <q-btn
                color="primary"
                padding="xs md xs sm"
                unelevated
                rounded
                :icon="huGuang.icon"
                :label="huGuang.label"
                :loading="huGuang.loading"
                @click="guanZhu"
              >
                <template #loading>
                  <q-spinner-gears color="white" size="sm" />
                </template>
              </q-btn>
            </q-toolbar>
            <!-- 点赞 + 收藏 + 贡献值 -->
            <div class="mini-heriz relative-position row">
              <!-- 贡献值 -->
              <q-item
                class="col justify-center"
                clickable
                v-ripple="{ color: 'blue' }"
                @click="useGloabStore.setDialog(0)"
              >
                <div class="column items-center">
                  <q-icon size="md">
                    <svg
                      t="1711115225499"
                      class="icon"
                      viewBox="0 0 1024 1024"
                      version="1.1"
                      xmlns="http://www.w3.org/2000/svg"
                      p-id="36808"
                      width="200"
                      height="200"
                    >
                      <path
                        d="M653.948758 680.106922c30.543344-31.116543 34.023484-28.414317 77.545702-40.287736a81.230556 81.230556 0 0 0 56.91052-57.934091c14.944129-56.746749 11.054562-49.909298 51.874554-91.46626 20.307639-20.676124 28.250546-50.850983 20.798953-79.101529-14.903187-56.746749-14.944129-48.844785 0-105.632477a82.950154 82.950154 0 0 0-20.798953-79.101529c-40.819992-41.556963-36.930424-34.719512-51.874554-91.466261a81.230556 81.230556 0 0 0-56.91052-57.934091c-55.764121-15.230729-49.049499-11.259276-89.869491-52.816238a79.5519 79.5519 0 0 0-77.791358-21.208381c-55.682236 15.189786-47.9031 15.230729-103.708164 0a79.510957 79.510957 0 0 0-77.791359 21.208381c-40.779049 41.556963-34.064427 37.585509-89.787605 52.816238A81.230556 81.230556 0 0 0 235.635962 135.076096c-14.944129 56.746749-11.095504 49.950241-51.915496 91.466261a82.868268 82.868268 0 0 0-20.798953 79.101529c14.903187 56.705806 14.944129 48.803842 0 105.632477-7.451593 28.250546 0.491314 58.425405 20.798953 79.101529 40.819992 41.556963 36.930424 34.719512 51.874554 91.466261 7.451593 28.250546 29.151288 50.359669 56.91052 57.93409 44.750503 12.20096 48.107814 10.276648 77.545702 40.287736a79.429072 79.429072 0 0 0 99.45011 11.627761 79.306243 79.306243 0 0 1 84.997296 0 79.429072 79.429072 0 0 0 99.45011-11.627761z m-330.572332-327.951992c0-106.041905 84.465038-191.939942 188.623574-191.939942s188.582631 85.93898 188.582631 191.939942c0 106.041905-84.424096 191.939942-188.582631 191.939942s-188.623574-85.93898-188.623574-191.939942z"
                        fill="#4499F5"
                        p-id="36809"
                      ></path>
                      <path
                        d="M322.311913 725.348738c-17.359756-17.400699-8.311393-12.48756-50.195898-23.705893-19.038411-5.117853-35.743082-14.903187-50.850983-26.612833L130.576685 897.390469a31.976343 31.976343 0 0 0 30.829944 44.054475l105.345877-4.012396 72.468792 76.522131a31.976343 31.976343 0 0 0 52.816238-9.908163l104.035707-255.155656a145.101355 145.101355 0 0 1-70.585422 19.16124c-38.977565 0-75.621389-15.148844-103.175908-42.703362zM893.423315 897.431412l-90.688347-222.4014c-15.148844 11.750589-31.853514 21.494981-50.850983 26.612833-42.130162 11.259276-32.877085 6.346137-50.195898 23.705893a145.01947 145.01947 0 0 1-103.175908 42.703362 145.347012 145.347012 0 0 1-70.626365-19.16124l104.035707 255.155656c8.802706 21.494981 36.848538 26.776605 52.816238 9.908163l72.468792-76.563074 105.38682 4.053339a31.976343 31.976343 0 0 0 30.829944-44.054475z"
                        fill="#4499F5"
                        opacity=".3"
                        p-id="36810"
                      ></path>
                    </svg>
                  </q-icon>
                  22
                </div>
              </q-item>

              <!-- star_outline star 收藏 -->
              <q-item
                class="col justify-center"
                clickable
                v-ripple="{ color: 'orange' }"
                @click="useGloabStore.setDialog(1)"
              >
                <div class="column items-center">
                  <q-icon
                    :name="useGloabStore.statIconAndColor.icon"
                    :color="useGloabStore.statIconAndColor.color"
                    size="md"
                  />
                  12
                </div>
              </q-item>

              <!-- favorite favorite_border 喜欢  -->
              <q-item
                class="col justify-center"
                clickable
                @click.stop="useGloabStore.contentUpClick"
                v-ripple="{ color: 'red' }"
              >
                <div class="column items-center">
                  <q-icon
                    :name="useGloabStore.upIconOrColor.icon"
                    :color="useGloabStore.upIconOrColor.color"
                    size="md"
                  />
                  12
                </div>
              </q-item>
            </div>

            <!-- 悬赏人员 -->
            <div class="row text-center justify-between no-wrap">
              <!-- 贡献值数量 -->
              <q-item
                clickable
                v-ripple
                class="inline-block q-pr-none"
                @click="openDialog('showGx')"
              >
                <q-icon size="md">
                  <svg
                    t="1711115225499"
                    class="icon"
                    viewBox="0 0 1024 1024"
                    version="1.1"
                    xmlns="http://www.w3.org/2000/svg"
                    p-id="36808"
                    width="200"
                    height="200"
                  >
                    <path
                      d="M653.948758 680.106922c30.543344-31.116543 34.023484-28.414317 77.545702-40.287736a81.230556 81.230556 0 0 0 56.91052-57.934091c14.944129-56.746749 11.054562-49.909298 51.874554-91.46626 20.307639-20.676124 28.250546-50.850983 20.798953-79.101529-14.903187-56.746749-14.944129-48.844785 0-105.632477a82.950154 82.950154 0 0 0-20.798953-79.101529c-40.819992-41.556963-36.930424-34.719512-51.874554-91.466261a81.230556 81.230556 0 0 0-56.91052-57.934091c-55.764121-15.230729-49.049499-11.259276-89.869491-52.816238a79.5519 79.5519 0 0 0-77.791358-21.208381c-55.682236 15.189786-47.9031 15.230729-103.708164 0a79.510957 79.510957 0 0 0-77.791359 21.208381c-40.779049 41.556963-34.064427 37.585509-89.787605 52.816238A81.230556 81.230556 0 0 0 235.635962 135.076096c-14.944129 56.746749-11.095504 49.950241-51.915496 91.466261a82.868268 82.868268 0 0 0-20.798953 79.101529c14.903187 56.705806 14.944129 48.803842 0 105.632477-7.451593 28.250546 0.491314 58.425405 20.798953 79.101529 40.819992 41.556963 36.930424 34.719512 51.874554 91.466261 7.451593 28.250546 29.151288 50.359669 56.91052 57.93409 44.750503 12.20096 48.107814 10.276648 77.545702 40.287736a79.429072 79.429072 0 0 0 99.45011 11.627761 79.306243 79.306243 0 0 1 84.997296 0 79.429072 79.429072 0 0 0 99.45011-11.627761z m-330.572332-327.951992c0-106.041905 84.465038-191.939942 188.623574-191.939942s188.582631 85.93898 188.582631 191.939942c0 106.041905-84.424096 191.939942-188.582631 191.939942s-188.623574-85.93898-188.623574-191.939942z"
                      fill="#4499F5"
                      p-id="36809"
                    ></path>
                    <path
                      d="M322.311913 725.348738c-17.359756-17.400699-8.311393-12.48756-50.195898-23.705893-19.038411-5.117853-35.743082-14.903187-50.850983-26.612833L130.576685 897.390469a31.976343 31.976343 0 0 0 30.829944 44.054475l105.345877-4.012396 72.468792 76.522131a31.976343 31.976343 0 0 0 52.816238-9.908163l104.035707-255.155656a145.101355 145.101355 0 0 1-70.585422 19.16124c-38.977565 0-75.621389-15.148844-103.175908-42.703362zM893.423315 897.431412l-90.688347-222.4014c-15.148844 11.750589-31.853514 21.494981-50.850983 26.612833-42.130162 11.259276-32.877085 6.346137-50.195898 23.705893a145.01947 145.01947 0 0 1-103.175908 42.703362 145.347012 145.347012 0 0 1-70.626365-19.16124l104.035707 255.155656c8.802706 21.494981 36.848538 26.776605 52.816238 9.908163l72.468792-76.563074 105.38682 4.053339a31.976343 31.976343 0 0 0 30.829944-44.054475z"
                      fill="#4499F5"
                      opacity=".3"
                      p-id="36810"
                    ></path>
                  </svg>
                </q-icon>
                <q-item-label class="full-width q-pt-xs"> + 12 </q-item-label>
              </q-item>

              <!-- 贡献值赠送成员 -->
              <q-item
                class="inline-block q-px-none cursor-pointer"
                v-for="i in 6"
                @click="openDialog('showGx')"
                clickable
              >
                <q-avatar size="md">
                  <q-img src="/avatar.jpeg">
                    <template v-slot:loading>
                      <q-spinner-orbit color="primary" size="xs" />
                    </template>
                  </q-img>

                  <!-- 用户认证 -->
                  <q-icon
                    v-if="i === 1"
                    class="absolute-bottom-right ren-zheng"
                  >
                    <svg
                      t="1709780406929"
                      class="icon"
                      viewBox="0 0 1024 1024"
                      version="1.1"
                      xmlns="http://www.w3.org/2000/svg"
                      p-id="4742"
                      width="200"
                      height="200"
                    >
                      <path
                        d="M855.458909 138.379636a622.312727 622.312727 0 0 1-197.818182-48.546909c-37.608727-16.989091-95.883636-55.854545-129.861818-80.058182a51.153455 51.153455 0 0 0-58.274909 0c-33.978182 24.203636-92.206545 63.069091-131.072 80.058182a622.312727 622.312727 0 0 1-197.818182 48.546909C113.943273 139.589818 93.277091 162.629818 93.277091 189.346909v305.803636a456.657455 456.657455 0 0 0 214.807273 387.165091l94.673454 59.485091 67.956364 42.496a52.596364 52.596364 0 0 0 53.434182 0l67.956363-42.496 94.673455-59.485091a456.657455 456.657455 0 0 0 214.807273-387.118545V188.136727a47.662545 47.662545 0 0 0-46.126546-49.757091z"
                        fill="#45BE89"
                        p-id="4743"
                      ></path>
                      <path
                        d="M498.641455 638.370909a34.909091 34.909091 0 0 1-25.460364 10.938182h-3.677091a38.306909 38.306909 0 0 1-26.670545-15.778909l-134.749091-133.492364c-10.891636-12.101818-15.732364-36.398545 1.256727-53.387636 15.778909-14.568727 40.029091-12.148364 58.228364 6.050909l111.662545 103.144727 200.238545-200.238545a37.794909 37.794909 0 0 1 52.177455 1.210182c10.938182 14.568727 10.938182 37.655273-2.420364 50.967272l-230.586181 230.586182z"
                        fill="#FFFFFF"
                        p-id="4744"
                      ></path>
                    </svg>
                  </q-icon>
                </q-avatar>

                <q-item-label
                  class="full-width q-pt-xs"
                  :class="i === 1 ? 'text-red' : ''"
                >
                  12
                </q-item-label>
              </q-item>

              <!-- 查看贡献值赠送信息 -->
              <q-btn
                color="grey"
                dense
                class="q-pr-sm"
                flat
                icon="more_horiz "
                @click="openDialog('showGx')"
                stretch
              />
            </div>

            <!-- 发布日期+编辑  -->
            <q-item class="mini-heriz q-pt-xs q-pb-none" style="min-height: 0">
              <q-item-section>
                <q-item-label caption>
                  <!-- 发布日期 -->
                  <span><q-icon name="access_time" />&nbsp;2024/1/2</span>
                  <!-- 浏览量-->
                  <span class="q-pl-md">
                    <q-icon name="o_remove_red_eye" />&nbsp;152</span
                  >
                  <!-- 评论数-->
                  <span class="q-pl-md">
                    <q-icon name="chat_bubble_outline" />&nbsp;12</span
                  >
                </q-item-label>
              </q-item-section>

              <q-item-section side>
                <q-btn icon="tune" color="grey" size="sm" flat>
                  <q-menu>
                    <q-list>
                      <q-item
                        clickable
                        v-close-popup
                        class="items-center"
                        @click="openDialog('videoEdit')"
                      >
                        <q-icon name="edit_road" size="xs" color="blue" />
                        <q-item-section class="q-pl-sm"> 编辑 </q-item-section>
                      </q-item>
                      <q-item
                        clickable
                        v-close-popup
                        class="items-center"
                        @click="delVideo"
                      >
                        <q-icon name="delete_outline" color="red" size="xs" />
                        <q-item-section class="q-pl-sm"> 删除 </q-item-section>
                      </q-item>
                      <q-separator />

                      <q-item
                        clickable
                        v-close-popup
                        class="items-center"
                        color="wargin"
                        @click="clockOrLockVideo"
                      >
                        <!-- lock warning | lock_open positive-->
                        <q-icon name="lock" size="xs" color="warning" />
                        <q-item-section class="q-pl-sm"> 锁定 </q-item-section>
                      </q-item>
                    </q-list>
                  </q-menu>
                </q-btn>
              </q-item-section>
            </q-item>

            <!-- 视频标题+简介 -->
            <q-item class="text-body1 q-pt-none">
              <q-item-section>
                <q-item-label class="text-weight-medium"
                  >标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</q-item-label
                >
              </q-item-section>
            </q-item>

            <q-item class="text-body1 q-pt-none q-pb-md">
              <!-- 简介 -->
              <q-item-section class="q-pt-none no-margin flex">
                <q-item-label ref="textoverEl" :lines="textOverCfg.active">
                  视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介视频简介
                </q-item-label>
              </q-item-section>

              <q-item-section side class="no-padding self-end">
                <q-checkbox
                  v-if="!textOverCfg.dis"
                  v-model="textOverCfg.show"
                  dense
                  color="blue-grey"
                  checked-icon="arrow_drop_up"
                  unchecked-icon="arrow_drop_down"
                  @update:model-value="textOverOpenClick(textOverCfg)"
                />
              </q-item-section>
            </q-item>
          </div>
        </q-card-section>
        <q-separator vertical v-if="Screen.gt.sm" />

        <!-- 评论   -->
        <q-card-section class="no-padding full-width">
          <!-- 评论区 -->
          <!-- 评论区 -->
          <!-- 评论区  -->
          <q-infinite-scroll
            v-if="comments.length"
            @load="commentLoad"
            :offset="250"
            :class="Screen.gt.sm ? 'overflow-auto window-height' : ''"
            :style="{ paddingBottom: '52px' }"
          >
            <!-- 评论排序 -->
            <q-card
              flat
              :class="Screen.gt.sm ? 'mini-heriz-bottom' : 'mini-heriz'"
              ref="commentBoxEl"
            >
              <q-item class="q-py-none">
                <q-item-section>
                  <q-item-label>
                    {{ videoInfo.sort ? "最新评论" : "热门评论" }}
                  </q-item-label>
                </q-item-section>

                <q-item-section avatar>
                  <q-checkbox
                    color="primary"
                    keep-color
                    :label="videoInfo.sort ? '按时间' : '安热度'"
                    v-model="videoInfo.sort"
                    checked-icon="access_time"
                    unchecked-icon="favorite_border"
                    @update:model-value="commentSort"
                  />
                </q-item-section>
              </q-item>
            </q-card>

            <transition-group
              appear
              enter-active-class="animated bounceInLeft slow"
              leave-active-class="animated backOutRight slow"
            >
              <div
                v-for="(commetn, commenI) in comments"
                :key="commetn.parentId"
              >
                <comment
                  :comments="commetn"
                  @loadChildComments="loadChildComments"
                  @delComment="delComment"
                  @commentReplyEv="commentReplyEv"
                />
              </div>
            </transition-group>

            <template v-slot:loading>
              <div class="row justify-center q-my-sm text-grey items-center">
                <q-spinner-comment color="primary" size="md" />
                <span>加载中...</span>
              </div>
            </template>

            <!-- 滚动结束 -->
            <div
              class="row justify-center q-my-md items-center q-gutter-x-md"
              v-if="commentLoadFinish"
            >
              <q-separator class="col" />
              <div class="col-auto text-grey">哎呀，到底儿了~</div>
              <q-separator class="col" />
            </div>
          </q-infinite-scroll>
          <div v-else class="flex justify-center items-center q-py-md">
            <q-spinner-comment color="primary" size="md" />评论正以光束赶来...
          </div>
        </q-card-section>
      </q-card-section>
    </q-card>

    <!--  评论输入框组件 -->

    <replay :big-screen="videoInfoHeight.w" />

    <!-- 弹窗   贡献值赠送列表|编辑视频  -->
    <q-dialog
      v-model="videoInfo.showDialog"
      :transition-hide="videoInfo.dialogwHide"
      :transition-show="videoInfo.dialogShow"
      :position="videoInfo.dialogPos"
      @show="useGloabStore.giveGxianLishShoeEv"
      @hide="useGloabStore.giveGxianLishHideEv"
    >
      <q-card style="max-width: 400px" class="full-width">
        <!-- 编辑视频 -->
        <q-card-section v-if="videoInfo.dialogType === 'videoEdit'">
          <q-card-section>
            <div class="text-body1 light-show">编辑</div>
          </q-card-section>

          <q-card-section class="q-pt-none">
            <q-input
              v-model="videoInfo.videoTitle"
              type="text"
              label="标题"
              :maxlength="120"
              outlined
              :input-style="{ minWidth: '350px' }"
            />
          </q-card-section>
          <q-card-section class="q-pt-none">
            <q-input
              v-model="videoInfo.videoContent"
              type="textarea"
              label="视频内容"
              outlined
              autogrow
              :maxlength="1000"
              :input-style="{ maxHeight: '90px' }"
            />
          </q-card-section>

          <q-card-actions
            :align="'right'"
            class="mini-heriz relative-position q-pb-none"
          >
            <q-btn
              label="取消"
              flat
              @click="videoInfo.showDialog = true"
              padding="xs md"
              v-close-popup
            />
            <q-btn
              label="提交"
              unelevated
              color="primary"
              @click="editVideoSub"
              padding="xs md"
            />
          </q-card-actions>
        </q-card-section>

        <!-- 贡献值赠送列表 -->
        <div v-else>
          <q-toolbar class="mini-heriz-bottom">
            <span class="light-show">赠送列表</span>
          </q-toolbar>

          <q-scroll-area style="height: 300px" class="an-parent">
            <q-infinite-scroll
              @load="useGloabStore.loadGiveGxList"
              :offset="50"
            >
              <q-item
                v-for="i in useGloabStore.gxList"
                ref="giveGxianListEl"
                class="op-0"
              >
                <!-- 头像 -->
                <q-item-section top avatar>
                  <q-avatar class="cursor-pointer" size="md">
                    <q-img src="/avatar.jpeg">
                      <template v-slot:loading>
                        <q-spinner-orbit color="primary" size="sm" />
                      </template>
                    </q-img>
                    <!-- 用户认证 -->
                    <q-icon class="absolute-bottom-right ren-zheng">
                      <svg
                        t="1709780406929"
                        class="icon"
                        viewBox="0 0 1024 1024"
                        version="1.1"
                        xmlns="http://www.w3.org/2000/svg"
                        p-id="4742"
                        width="200"
                        height="200"
                      >
                        <path
                          d="M855.458909 138.379636a622.312727 622.312727 0 0 1-197.818182-48.546909c-37.608727-16.989091-95.883636-55.854545-129.861818-80.058182a51.153455 51.153455 0 0 0-58.274909 0c-33.978182 24.203636-92.206545 63.069091-131.072 80.058182a622.312727 622.312727 0 0 1-197.818182 48.546909C113.943273 139.589818 93.277091 162.629818 93.277091 189.346909v305.803636a456.657455 456.657455 0 0 0 214.807273 387.165091l94.673454 59.485091 67.956364 42.496a52.596364 52.596364 0 0 0 53.434182 0l67.956363-42.496 94.673455-59.485091a456.657455 456.657455 0 0 0 214.807273-387.118545V188.136727a47.662545 47.662545 0 0 0-46.126546-49.757091z"
                          fill="#45BE89"
                          p-id="4743"
                        ></path>
                        <path
                          d="M498.641455 638.370909a34.909091 34.909091 0 0 1-25.460364 10.938182h-3.677091a38.306909 38.306909 0 0 1-26.670545-15.778909l-134.749091-133.492364c-10.891636-12.101818-15.732364-36.398545 1.256727-53.387636 15.778909-14.568727 40.029091-12.148364 58.228364 6.050909l111.662545 103.144727 200.238545-200.238545a37.794909 37.794909 0 0 1 52.177455 1.210182c10.938182 14.568727 10.938182 37.655273-2.420364 50.967272l-230.586181 230.586182z"
                          fill="#FFFFFF"
                          p-id="4744"
                        ></path>
                      </svg>
                    </q-icon>
                  </q-avatar>
                </q-item-section>

                <!-- 昵称 -->
                <q-item-section>
                  <q-item-label :lines="1">昵称昵称昵称昵称</q-item-label>
                  <q-item-label caption lines="2">2024/2/4</q-item-label>
                </q-item-section>
                <q-item-section side top>
                  <q-item-label>×&nbsp;10</q-item-label>
                </q-item-section>
              </q-item>
              <template v-slot:loading>
                <div class="row justify-center q-my-md">
                  <q-spinner-bars color="primary" size="sm" />
                </div>
              </template>
            </q-infinite-scroll>
          </q-scroll-area>
        </div>
      </q-card>
    </q-dialog>

    <q-page-scroller
      v-if="Screen.lt.md"
      position="bottom-right"
      :scroll-offset="150"
      :offset="[18, 30]"
    >
      <q-btn fab-mini icon="keyboard_arrow_up" color="primary" />
    </q-page-scroller>
  </q-page>
</template>
  
<script setup>
import Replay from "../components/Replay.vue";
import { onBeforeMount, onMounted, ref } from "vue";
import Player from "xgplayer";
import "xgplayer/dist/index.min.css";
import {
  commentSort,
  videoInfo,
  guanZhu,
  giveGonXian,
  editVideoSub,
  clockOrLockVideo,
  delVideo,
  openDialog,
  huGuang,
} from "../common/userView";
import { replyInfo, commentBoxEl, commentOnIn } from "../common/reply";
import axios from "axios";
import Comment from "../components/Comment.vue";
import {
  comments,
  loadChildComments,
  loadComments,
  scrolled,
  delComment,
  commentReplyEv,
} from "../common/comments";
import { Screen } from "quasar";
import { textOverInit, textOverOpenClick } from "../utils/textOver";
import gloabStore, { giveGxianListEl } from "../stores/starUpGxStrore";

const useGloabStore = gloabStore();

const textoverEl = ref();
const textOverCfg = ref({
  active: 2,
  show: false,
  lines: 2,
  dis: false,
  once: true,
  isOver: false,
});

const commentLoadFinish = ref(false);
const videoInfoHeight = ref({
  w: 0,
  h: 0,
});

// 监听视频高度
const onResize = (val) => {
  if (Screen.lt.sm) return;
  videoInfoHeight.value = {
    h: `calc(100vh - ${val.height}px - 16px)`,
    w: `calc(100vw - ${val.width}px - 233px)`,
  };

};

// 评论加载
const commentLoad = async (index, done) => {
  const { data } = await axios("/api/comments", {
    params: {
      t: 0,
      page: 1,
      count: 2,
    },
  });
  if (data) {
    // 设置加载子评论按钮状态
    loadComments(data);
    done();
  }
};

onBeforeMount(async () => {
  const { data } = await axios("/api/comments", {
    params: {
      t: 0,
      page: 1,
      count: 2,
    },
  });

  // 设置加载子评论按钮状态
  loadComments(data);
});

const playerOpts = {
  id: "player", //元素id
  url: "/4.mp4", //视频地址
  poster: "/bg.jpeg", //封面图
  lang: "zh-cn", //设置中文
  lastPlayTime: 0, //视频起播时间（单位：秒）
  lastPlayTimeHideDelay: 5, //提示文字展示时长（单位：秒）
  closeVideoClick: false, // true - 禁止pc端单击暂停，反之
  closeVideoTouch: true, // true - 禁止移动端单击暂停，反之
  videoInit: true,
  // fluid: Screen.lt.md, //填满屏幕
  fitVideoSize: "auto",
  autoplay: false, //自动播放
  loop: false, //循环播放
  pip: true,
  volume: 1, //音量 0 -  1
  playbackRate: [0.5, 0.75, 1, 1.5, 2], //传入倍速可选数组
  // 删除控件
  // ignores: ['time', 'definition', 'error', 'fullscreen', 'i18n', 'loading', 'mobile', 'pc', 'play', 'poster', 'progress', 'replay', 'volume'],
  ignores: ["volume"],
  rotateFullscreen: true,
};

//  播放器
let player = null;

// 必须在onMounted 或 nextTick实例Xgplayer播放器
onMounted(() => {
  player = new Player(playerOpts);
  textOverInit(textoverEl.value, textOverCfg.value);
});
</script>

<style lang="sass">
</style>

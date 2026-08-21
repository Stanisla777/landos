<div class="test-n-slider" ref="ContainerSlider">
        <Slider
          ref="slider"
          :modelValue="dragValue"
          @update:modelValue="onValueChange"
          @dragstart="isDragging = true"
          @dragend="isDragging = false"
          :min="0"
          :max="optionsList.length - 1"
          :step="0.01"
          :tooltip="false"
          :height="4"
          :dot-size="24"
          :disabled="time_end"
          class="test-n-slider__track"
        />

        <div class="test-n-slider__marks" :class="{ 'is-disabled': time_end }">
          <div
            v-for="(option, index) in optionsList"
            :key="option.id"
            class="test-n-slider__mark"
            :class="{ 'is-active': index === selectedIndex }"
            :style="{ left: getEvenPosition(index) + '%' }"
            @click.stop="selectByIndex(index)"
          >
            <span class="test-n-slider__mark-label">{{ option.name }}</span>
          </div>
        </div>
      </div>

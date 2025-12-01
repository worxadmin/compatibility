<template>
  <div class="compatibilityCheck">
    <div class="container">
      <div class="icon-placeholder">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
          <path d="M11 15h2v2h-2zm0-8h2v6h-2zm1-5C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"/>
        </svg>
      </div>
      <h1>Browser Compatibility Checker</h1>
      <p>Check which modern browser features are supported by your current browser.</p>

      <div class="all-checks">
        <h2>Browser Feature Support:</h2>
        <ul>
          <li :class="checks.webGL2 ? 'passed' : 'failed'">
            <span class="icon">{{ checks.webGL2 ? '✓' : '✗' }}</span>
            <span class="label">WebGL2</span>
            <span class="status">{{ checks.webGL2 ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.webAudio ? 'passed' : 'failed'">
            <span class="icon">{{ checks.webAudio ? '✓' : '✗' }}</span>
            <span class="label">Web Audio</span>
            <span class="status">{{ checks.webAudio ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.localStorage ? 'passed' : 'failed'">
            <span class="icon">{{ checks.localStorage ? '✓' : '✗' }}</span>
            <span class="label">Local Storage</span>
            <span class="status">{{ checks.localStorage ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.promises ? 'passed' : 'failed'">
            <span class="icon">{{ checks.promises ? '✓' : '✗' }}</span>
            <span class="label">Promises</span>
            <span class="status">{{ checks.promises ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.transforms ? 'passed' : 'failed'">
            <span class="icon">{{ checks.transforms ? '✓' : '✗' }}</span>
            <span class="label">CSS Transforms</span>
            <span class="status">{{ checks.transforms ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.transitions ? 'passed' : 'failed'">
            <span class="icon">{{ checks.transitions ? '✓' : '✗' }}</span>
            <span class="label">CSS Transitions</span>
            <span class="status">{{ checks.transitions ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.animations ? 'passed' : 'failed'">
            <span class="icon">{{ checks.animations ? '✓' : '✗' }}</span>
            <span class="label">CSS Animations</span>
            <span class="status">{{ checks.animations ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.grid ? 'passed' : 'failed'">
            <span class="icon">{{ checks.grid ? '✓' : '✗' }}</span>
            <span class="label">CSS Grid</span>
            <span class="status">{{ checks.grid ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.variables ? 'passed' : 'failed'">
            <span class="icon">{{ checks.variables ? '✓' : '✗' }}</span>
            <span class="label">CSS Variables</span>
            <span class="status">{{ checks.variables ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.flexbox ? 'passed' : 'failed'">
            <span class="icon">{{ checks.flexbox ? '✓' : '✗' }}</span>
            <span class="label">Flexbox</span>
            <span class="status">{{ checks.flexbox ? 'Supported' : 'Not Supported' }}</span>
          </li>
          <li :class="checks.serviceWorkers ? 'passed' : 'failed'">
            <span class="icon">{{ checks.serviceWorkers ? '✓' : '✗' }}</span>
            <span class="label">Service Workers</span>
            <span class="status">{{ checks.serviceWorkers ? 'Supported' : 'Not Supported' }}</span>
          </li>
        </ul>
      </div>

      <div class="summary">
        <div class="summary-stat">
          <span class="number">{{ passedCount }}</span>
          <span class="text">Features Supported</span>
        </div>
        <div class="summary-stat">
          <span class="number">{{ failedCount }}</span>
          <span class="text">Features Not Supported</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, computed } from 'vue';

export default {
  name: 'CompatibilityChecker',
  setup() {
    const checks = ref({
      webGL2: false,
      webAudio: false,
      localStorage: false,
      promises: false,
      transforms: false,
      transitions: false,
      animations: false,
      grid: false,
      variables: false,
      flexbox: false,
      serviceWorkers: false
    });

    const passedCount = computed(() => {
      return Object.values(checks.value).filter(check => check).length;
    });

    const failedCount = computed(() => {
      return Object.values(checks.value).filter(check => !check).length;
    });

    onMounted(() => {
      checkBrowserFeatures();
    });

    function checkBrowserFeatures() {
      checks.value.webGL2 = supportsWebGL2();
      checks.value.webAudio = supportsWebAudio();
      checks.value.localStorage = supportsLocalStorage();
      checks.value.promises = supportsPromises();
      checks.value.transforms = supportsTransforms();
      checks.value.transitions = supportsTransitions();
      checks.value.animations = supportsAnimations();
      checks.value.grid = supportsCSSGrid();
      checks.value.variables = supportsCSSVariables();
      checks.value.flexbox = supportsFlexbox();
      checks.value.serviceWorkers = supportsServiceWorkers();
    }

    function supportsWebGL2() {
      try {
        const canvas = document.createElement('canvas');
        return !!window.WebGL2RenderingContext && (canvas.getContext('webgl2'));
      } catch (e) {
        return false;
      }
    }

    function supportsWebAudio() {
      return !!(window.AudioContext || window.webkitAudioContext);
    }

    function supportsLocalStorage() {
      try {
        const test = '__test__';
        localStorage.setItem(test, test);
        localStorage.removeItem(test);
        return true;
      } catch(e) {
        return false;
      }
    }

    function supportsPromises() {
      return 'Promise' in window;
    }

    function supportsTransforms() {
      return 'transform' in document.body.style;
    }

    function supportsTransitions() {
      return 'transition' in document.body.style;
    }

    function supportsAnimations() {
      return 'animation' in document.body.style;
    }

    function supportsCSSGrid() {
      return CSS.supports('display', 'grid');
    }

    function supportsCSSVariables() {
      return CSS.supports('color', 'var(--fake-var)');
    }

    function supportsFlexbox() {
      return CSS.supports('display', 'flex');
    }

    function supportsServiceWorkers() {
      return 'serviceWorker' in navigator;
    }

    return { checks, passedCount, failedCount };
  }
}
</script>

<style lang="scss" scoped>
.compatibilityCheck {
  display: table;
  width: 100%;
  min-height: 100vh;
  text-align: center;
  padding: 40px 20px;

  .container {
    display: table-cell;
    vertical-align: middle;
    padding: 20px;
    margin: auto;
    text-align: center;

    .icon-placeholder {
      width: 64px;
      height: 64px;
      margin: 0 auto 20px;
      color: #3F8CBA;

      svg {
        width: 100%;
        height: 100%;
      }
    }

    h1 {
      margin: 0 0 10px 0;
      font-size: 28px;
      color: #3F8CBA;
      font-weight: 600;
    }

    > p {
      margin: 10px auto 30px;
      color: #4D4D4D;
      max-width: 400px;
      font-size: 16px;
      line-height: 1.5;
    }

    .all-checks {
      margin: 30px auto;
      max-width: 600px;

      h2 {
        font-size: 20px;
        color: #3F8CBA;
        margin-bottom: 20px;
        font-weight: 600;
      }

      ul {
        list-style: none;
        padding: 0;
        margin: 0;

        li {
          display: flex;
          align-items: center;
          padding: 12px 20px;
          margin: 8px 0;
          border-radius: 8px;
          font-size: 15px;
          transition: all 0.2s ease;

          .icon {
            font-size: 18px;
            font-weight: bold;
            width: 30px;
            flex-shrink: 0;
          }

          .label {
            flex: 1;
            text-align: left;
            font-weight: 500;
          }

          .status {
            font-size: 13px;
            opacity: 0.8;
          }

          &.passed {
            background-color: #e8f5e9;
            border-left: 4px solid #4caf50;
            color: #2e7d32;

            .icon {
              color: #4caf50;
            }
          }

          &.failed {
            background-color: #ffebee;
            border-left: 4px solid #f44336;
            color: #c62828;

            .icon {
              color: #f44336;
            }
          }
        }
      }
    }

    .summary {
      display: flex;
      justify-content: center;
      gap: 40px;
      margin-top: 40px;
      flex-wrap: wrap;

      .summary-stat {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 20px 30px;
        background: white;
        border-radius: 12px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);

        .number {
          font-size: 36px;
          font-weight: bold;
          color: #3F8CBA;
          margin-bottom: 8px;
        }

        .text {
          font-size: 14px;
          color: #666;
          text-transform: uppercase;
          letter-spacing: 0.5px;
        }
      }
    }
  }
}
</style>

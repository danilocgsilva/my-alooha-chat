## Question

I am developing a Vue.js application with Tailwind.
My index page will emulate current AI chat interfaces. Create a visually appealing Tailwind CSS template with a familiar UX, featuring a theme switcher (dark/light mode) in the header.

## Answer

# Vue.js + Tailwind AI Chat Interface Template

Here's a complete template for an AI chat interface with dark/light theme switching:

```html
<template>
  <div class="min-h-screen bg-gray-50 dark:bg-gray-900 transition-colors duration-200">
    <!-- Header -->
    <header class="sticky top-0 z-10 bg-white/80 dark:bg-gray-800/80 backdrop-blur-sm border-b border-gray-200 dark:border-gray-700">
      <div class="max-w-4xl mx-auto px-4 py-3 flex justify-between items-center">
        <h1 class="text-xl font-semibold text-gray-800 dark:text-white">AI Assistant</h1>
        <div class="flex items-center space-x-2">
          <button
            @click="toggleTheme"
            class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors"
            aria-label="Toggle theme"
          >
            <svg
              v-if="isDarkMode"
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 text-yellow-400"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"
              />
            </svg>
            <svg
              v-else
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 text-gray-600 dark:text-gray-300"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"
              />
            </svg>
          </button>
        </div>
      </div>
    </header>

    <!-- Chat Container -->
    <main class="max-w-4xl mx-auto px-4 py-6 flex-1">
      <div class="flex flex-col h-[calc(100vh-120px)] overflow-hidden">
        <!-- Messages -->
        <div
          ref="messagesContainer"
          class="flex-1 overflow-y-auto space-y-4 pb-4"
          @scroll="handleScroll"
        >
          <div
            v-for="(message, index) in messages"
            :key="index"
            class="flex flex-col gap-2 max-w-[80%]"
            :class="{
              'ml-auto': message.sender === 'user',
              'mr-auto': message.sender !== 'user'
            }"
          >
            <div
              class="px-4 py-2 rounded-lg"
              :class="{
                'bg-blue-500 text-white rounded-br-none': message.sender === 'user',
                'bg-gray-100 dark:bg-gray-700 text-gray-800 dark:text-gray-200 rounded-bl-none': message.sender !== 'user'
              }"
            >
              {{ message.text }}
            </div>
            <div
              class="text-xs flex justify-end items-center gap-1"
              :class="{
                'text-blue-500 ml-auto': message.sender === 'user',
                'text-gray-400 dark:text-gray-500 mr-auto': message.sender !== 'user'
              }"
            >
              {{ formatTime(message.timestamp) }}
              <span v-if="message.sender !== 'user'" class="flex items-center gap-1">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-3 w-3"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13"
                  />
                </svg>
              </span>
            </div>
          </div>
        </div>

        <!-- Input Area -->
        <div class="mt-4">
          <div class="flex items-center p-2 bg-white dark:bg-gray-800 rounded-lg border border-gray-200 dark:border-gray-700">
            <button
              class="p-2 text-gray-500 hover:text-gray-700 dark:hover:text-gray-300 transition-colors"
              aria-label="Attach file"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13" />
              </svg>
            </button>
            <input
              v-model="newMessage"
              @keyup.enter="sendMessage"
              type="text"
              placeholder="Type your message..."
              class="flex-1 bg-transparent outline-none text-gray-800 dark:text-white placeholder-gray-400 dark:placeholder-gray-500"
            />
            <button
              @click="sendMessage"
              :disabled="!newMessage.trim()"
              class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors disabled:opacity-50"
              aria-label="Send message"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-blue-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </main>

    <!-- Typing indicator (shown when AI is responding) -->
    <div
      v-if="isTyping"
      class="max-w-4xl mx-auto px-4 pb-6 flex items-center gap-2 text-gray-500 dark:text-gray-400"
    >
      <span class="flex space-x-1">
        <span class="w-1 h-1 bg-current rounded-full animate-bounce"></span>
        <span class="w-1 h-1 bg-current rounded-full animate-bounce" style="animation-delay: 0.2s;"></span>
        <span class="w-1 h-1 bg-current rounded-full animate-bounce" style="animation-delay: 0.4s;"></span>
      </span>
      <span class="ml-2 text-sm">AI is typing...</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      messages: [
        { sender: 'assistant', text: "Hello! How can I help you today?", timestamp: new Date() },
      ],
      newMessage: '',
      isTyping: false,
      isDarkMode: false,
      isAtBottom: true
    }
  },
  mounted() {
    // Load saved theme preference
    this.isDarkMode = localStorage.getItem('theme') === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches);
    this.applyTheme();
  },
  methods: {
    toggleTheme() {
      this.isDarkMode = !this.isDarkMode;
      this.applyTheme();
      localStorage.setItem('theme', this.isDarkMode ? 'dark' : 'light');
    },
    applyTheme() {
      if (this.isDarkMode) {
        document.documentElement.classList.add('dark');
      } else {
        document.documentElement.classList.remove('dark');
      }
    },
    sendMessage() {
      if (!this.newMessage.trim()) return;

      const userMessage = {
        sender: 'user',
        text: this.newMessage,
        timestamp: new Date()
      };

      this.messages.push(userMessage);
      this.newMessage = '';
      this.isTyping = true;
      this.scrollToBottom();

      // Simulate AI response
      setTimeout(() => {
        const aiResponse = {
          sender: 'assistant',
          text: "This is a simulated response to your message. In a real application, you would call an API here.",
          timestamp: new Date()
        };

        this.messages.push(aiResponse);
        this.isTyping = false;
        this.scrollToBottom();
      }, 1000);
    },
    scrollToBottom() {
      if (this.isAtBottom) {
        setTimeout(() => {
          this.$refs.messagesContainer.scrollTop = this.$refs.messagesContainer.scrollHeight;
        }, 100);
      }
    },
    handleScroll() {
      const container = this.$refs.messagesContainer;
      const isAtBottom = container.scrollHeight - container.scrollTop <= container.clientHeight + 50;
      this.isAtBottom = isAtBottom;
    },
    formatTime(date) {
      return date.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    }
  }
}
</script>

<style>
/* Custom scrollbar for better UX */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: #c1d5e0;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #a2b5c6;
}

.dark ::-webkit-scrollbar-thumb {
  background: #4a5568;
}

.dark ::-webkit-scrollbar-thumb:hover {
  background: #2d3748;
}
</style>
```

## Key Features

1. **Theme Switcher**:
   - Toggle between light and dark modes
   - Persists user preference using localStorage
   - Uses Tailwind's dark mode variant

2. **Chat Interface**:
   - User messages on the right (blue bubbles)
   - AI responses on the left (gray bubbles)
   - Timestamp display for each message
   - Typing indicator when AI is "responding"

3. **Responsive Design**:
   - Max-width container for better readability
   - Proper spacing and padding
   - Mobile-friendly layout

4. **UX Enhancements**:
   - Auto-scrolling to bottom of chat
   - Custom scrollbar styling
   - Input field with send button and file attachment option
   - Message bubbles with proper corner rounding

5. **Accessibility**:
   - ARIA labels for interactive elements
   - Proper contrast ratios in both themes
   - Keyboard navigation support (Enter to send)

To use this template, simply copy the code into your Vue component file. The template includes all necessary Tailwind classes and basic Vue.js functionality without requiring any external dependencies beyond what you're already using.

Would you like me to explain any specific part of this implementation in more detail?
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
          >
            <svg
              v-if="isDarkMode"
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 text-yellow-400"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"
              />
            </svg>
            <svg
              v-else
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 text-gray-700 dark:text-white"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707a1 1 0 001.414 0zm1.465 8.536l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zm-2.121-10.607l.708.707a1 1 0 001.414-1.414L6.465 3.05a1 1 0 00-1.414 1.414z"
              />
            </svg>
          </button>
        </div>
      </div>
    </header>

    <!-- Chat Container -->
    <main class="max-w-4xl mx-auto px-4 py-6 flex-1">
      <div
        id="chat-container"
        class="flex flex-col space-y-4 overflow-y-auto h-[calc(100vh-12rem)] pb-4"
      >
        <!-- Example messages -->
        <div class="flex justify-start">
          <div class="max-w-xs md:max-w-md lg:max-w-lg xl:max-w-xl rounded-lg p-3 bg-gray-100 dark:bg-gray-800 text-gray-900 dark:text-white">
            Hello! How can I help you today?
          </div>
        </div>

        <div class="flex justify-end">
          <div class="max-w-xs md:max-w-md lg:max-w-lg xl:max-w-xl rounded-lg p-3 bg-blue-500 text-white">
            I'd like to know about the latest AI developments
          </div>
        </div>

        <div class="flex justify-start">
          <div class="max-w-xs md:max-w-md lg:max-w-lg xl:max-w-xl rounded-lg p-3 bg-gray-100 dark:bg-gray-800 text-gray-900 dark:text-white space-y-2">
            <p>Here are some recent AI developments:</p>
            <ul class="list-disc list-inside space-y-1">
              <li>New language models with improved reasoning capabilities</li>
              <li>Advancements in computer vision for medical imaging</li>
              <li>Ethical AI frameworks being adopted by major tech companies</li>
            </ul>
          </div>
        </div>
      </div>

      <!-- Input Area -->
      <div class="mt-auto pt-4 border-t border-gray-200 dark:border-gray-700">
        <form @submit.prevent="sendMessage" class="flex space-x-2">
          <input
            v-model="newMessage"
            type="text"
            placeholder="Type your message..."
            class="flex-1 rounded-full py-2 px-4 bg-white dark:bg-gray-800 border border-gray-300 dark:border-gray-600 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-colors"
          />
          <button
            type="submit"
            class="p-2 rounded-full bg-blue-500 hover:bg-blue-600 text-white transition-colors disabled:opacity-50 disabled:cursor-not-allowed"
            :disabled="!newMessage.trim()"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
              <path d="M10.894 2.553a1 1 0 00-1.788 0l-7 14a1 1 0 001.169 1.409l5-1.429A1 1 0 009 15.571V11a1 1 0 112 0v4.571a1 1 0 00.725.962l5 1.428a1 1 0 001.17-1.408l-7-14z" />
            </svg>
          </button>
        </form>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newMessage: '',
      isDarkMode: false
    }
  },
  mounted() {
    // Check for saved theme preference or use system preference
    const savedTheme = localStorage.getItem('theme');
    if (savedTheme) {
      this.isDarkMode = savedTheme === 'dark';
    } else if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
      this.isDarkMode = true;
    }
    this.applyTheme();
  },
  methods: {
    toggleTheme() {
      this.isDarkMode = !this.isDarkMode;
      localStorage.setItem('theme', this.isDarkMode ? 'dark' : 'light');
      this.applyTheme();
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

      // Add user message to chat
      const userMessage = this.newMessage;
      this.newMessage = '';

      // In a real app, you would send the message to your backend here
      // For this template, we'll just simulate a response

      setTimeout(() => {
        // Simulate AI response
        const aiResponse = `I received: "${userMessage}". This is a simulated response.`;

        // Scroll to bottom of chat
        const chatContainer = document.getElementById('chat-container');
        chatContainer.scrollTop = chatContainer.scrollHeight;
      }, 500);
    }
  }
}
</script>

<style>
/* Custom scrollbar for better UX */
#chat-container::-webkit-scrollbar {
  width: 8px;
}

#chat-container::-webkit-scrollbar-track {
  background: #f1f1f1;
  dark-background: #374151;
}

#chat-container::-webkit-scrollbar-thumb {
  background: #888;
  dark-background: #6b7280;
  border-radius: 4px;
}

#chat-container::-webkit-scrollbar-thumb:hover {
  background: #555;
  dark-background: #4b5563;
}
</style>
```

## Key Features:

1. **Responsive Design**:
   - Works on mobile and desktop
   - Message bubbles adjust width based on screen size

2. **Theme Switching**:
   - Dark/light mode toggle in header
   - Persists user preference using localStorage
   - Respects system preference by default

3. **Chat Interface Elements**:
   - Scrollable chat container with custom scrollbar
   - User messages aligned right (blue background)
   - AI responses aligned left (gray background)
   - Input area with send button

4. **Visual Enhancements
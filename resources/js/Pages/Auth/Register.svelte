<script>
    import { useForm, Link } from '@inertiajs/svelte';
    import AuthLayout from '@/Components/Layouts/AuthLayout.svelte';
    import Button from '@/Components/UI/Button.svelte';
    import PasswordInput from '@/Components/UI/Form/PasswordInput.svelte';
    import EmailInput from '@/Components/UI/Form/EmailInput.svelte';
    import FormError from '@/Components/UI/Form/FormError.svelte';
    
    const form = useForm({
        name: '',
        email: '',
        password: '',
        password_confirmation: '',
    });
    
    function submit() {
        $form.post('/register', {
            onError: (errors) => {
                console.error('Register errors:', errors);
            },
        });
    }
</script>

<AuthLayout 
    title="Create your account"
    subtitle="Join us and start your journey"
>
    <!-- Error Alert -->
    {#if Object.keys($form.errors).length > 0}
        <div class="mb-6">
            <FormError errors={$form.errors} title="Registration failed" />
        </div>
    {/if}
    
    <!-- Form -->
    <form onsubmit={(e) => { e.preventDefault(); submit(); }} class="space-y-5">
        <!-- Name Field -->
        <div>
            <label for="name" class="block text-sm font-semibold text-gray-700 mb-2">
                Full Name
            </label>
            <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                    <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"/>
                    </svg>
                </div>
                <input
                    id="name"
                    type="text"
                    bind:value={$form.name}
                    placeholder="John Doe"
                    class="w-full pl-10 pr-4 py-2.5 border border-gray-300 rounded-lg transition-all duration-200 focus:outline-none focus:border-primary-500 focus:ring-2 focus:ring-primary-500/20 hover:border-gray-400 {$form.errors.name ? 'border-red-500 bg-red-50' : ''}"
                    required
                />
            </div>
        </div>
        
        <!-- Email Field -->
        <div>
            <label for="email" class="block text-sm font-semibold text-gray-700 mb-2">
                Email Address
            </label>
            <EmailInput
                id="email"
                bind:value={$form.email}
                error={$form.errors.email}
                placeholder="name@company.com"
                required
            />
        </div>
        
        <!-- Password Field -->
        <div>
            <label for="password" class="block text-sm font-semibold text-gray-700 mb-2">
                Password
            </label>
            <PasswordInput
                id="password"
                bind:value={$form.password}
                placeholder="Create a strong password"
                error={$form.errors.password}
                showStrength={true}
                autocomplete="new-password"
                required
            />
        </div>
        
        <!-- Confirm Password Field -->
        <div>
            <label for="password_confirmation" class="block text-sm font-semibold text-gray-700 mb-2">
                Confirm Password
            </label>
            <PasswordInput
                id="password_confirmation"
                bind:value={$form.password_confirmation}
                placeholder="Confirm your password"
                error={$form.errors.password_confirmation}
                autocomplete="new-password"
                required
            />
        </div>
        
        <!-- Terms Checkbox -->
        <div class="flex items-start pt-2">
            <input
                id="terms"
                type="checkbox"
                class="w-4 h-4 border-gray-300 rounded transition-colors mt-0.5"
                style="accent-color: var(--theme-primary-600)"
                required
            />
            <label for="terms" class="ml-2 text-sm text-gray-600">
                I agree to the 
                <a href="#" class="font-medium text-primary-600 hover:text-primary-700 transition-colors">Terms of Service</a> and 
                <a href="#" class="font-medium text-primary-600 hover:text-primary-700 transition-colors">Privacy Policy</a>
            </label>
        </div>
        
        <!-- Submit Button -->
        <div class="pt-2">
            <Button
                type="submit"
                variant="primary"
                size="lg"
                class="w-full"
                loading={$form.processing}
                disabled={$form.processing}
            >
                {#if $form.processing}
                    Creating account...
                {:else}
                    Create account
                {/if}
            </Button>
        </div>
    </form>
    
    <!-- Divider -->
    <div class="relative my-8">
        <div class="absolute inset-0 flex items-center">
            <div class="w-full border-t border-gray-200"></div>
        </div>
        <div class="relative flex justify-center text-sm">
            <span class="px-4 bg-white text-gray-500">Already have an account?</span>
        </div>
    </div>
    
    <!-- Login Link -->
    <Link 
        href="/login" 
        class="w-full flex items-center justify-center gap-2 px-4 py-3 border-2 border-gray-200 rounded-xl text-gray-700 font-medium hover:border-primary-300 hover:text-primary-600 hover:bg-primary-50/50 transition-all duration-200"
    >
        Sign in instead
    </Link>
</AuthLayout>

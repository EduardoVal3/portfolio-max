<template>
    <section id="projects">
      <div class="container">
        <h2>{{ $t('PROJECTS.TITLE') }}</h2>
  
        <div class="featured-project-cards">
          <FeaturedProjectCard
            v-for="(project, index) in featuredProjects"
            :key="index"
            :project="project"
            :reverse="index % 2 === 0"
          />
        </div>
  
        <div class="project-cards">
          <ProjectCard
            v-for="(project, index) in projects"
            :key="index"
            :project="project"
          />
        </div>
  
        <div class="coming-soon-message">
          <h1>{{ $t('PROJECTS.COMING_SOON')}}</h1>
        </div>
      </div>
    </section>
  </template>
  
  <script setup>
    import { gsap } from 'gsap';
  
    import { computed, onMounted } from 'vue';
    import { useI18n } from 'vue-i18n';
  
    import FeaturedProjectCard from '@/components/cards/FeaturedProjectCard.vue';
    import ProjectCard from '@/components/cards/ProjectCard.vue';
    
    const { t: $t } = useI18n();
    const featuredProjects = computed(() => [
      {
        name: 'PROJECT.Example',
        description: $t('PROJECTS.EJEMPLO'),
        stack: ['Example', 'Screenshot', 'Vue'],
        image: '/images/cap_ejemplo.png',
        link: '#',
        repository: '#',
      }
    ]);
  
    const projects = [];
  
    const addObserverOnScroll = () => {
      const observer = new IntersectionObserver((entries, self) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            animateElement();
            self.unobserve(entry.target);
          }
        });
      });
  
      const target = document.querySelector('#projects .container');
      observer.observe(target)
  
      function animateElement() {
        const tl = gsap.timeline();
  
        const title = document.querySelector('#projects h2');
        const featuredCards = document.querySelectorAll('.featured-project-cards > *');
        const message = document.querySelector('.coming-soon-message');
  
        gsap.fromTo(
          title, 
          { opacity: 0, y: 50, filter: 'blur(2px)' }, 
          { opacity: 1, y: 0, duration: .5, filter: 'blur(0px)', ease: 'back.out(1.4)', delay: .35 }
        );
  
        tl.fromTo(
          featuredCards, 
          { opacity: 0, y: 75, filter: 'blur(2px)' }, 
          { opacity: 1, y: 0, duration: .5, filter: 'blur(0px)', stagger: .25, delay: .5 }
        );
  
        gsap.fromTo(
          message, 
          { opacity: 0, y: 75, filter: 'blur(2px)' }, 
          { opacity: 1, y: 0, duration: .5, filter: 'blur(0px)', delay: .5 }
        );
      }
    };
  
    onMounted(addObserverOnScroll);
  </script>
  
  <style lang="scss" scoped>
    #projects {
      display: flex;
      justify-content: center;
      flex-direction: column;
  
      h2 {
        display: flex;
        align-items: center;
        font-size: 1.5rem;
        font-weight: 700;
        margin-bottom: 2.5rem;
        color: var(--secondary-text);
        // Opacity 0 to prevent the animation from running before the element is visible
        opacity: 0;
  
        &::before {
        content: '';
        display: block;
        position: relative;
        width: 50%;
        height: 1px;
        margin-right: 20px;
        background-color: var(--default-border);
      }
  
        &::after {
          content: '';
          display: block;
          position: relative;
          width: 50%;
          height: 1px;
          margin-left: 20px;
          background-color: var(--default-border);
        }
      }
  
      .featured-project-cards {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: start;
        flex-wrap: wrap;
        gap: 3rem;
        margin-top: 2rem;
      }
  
      .project-cards {
        margin-top: 3rem;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 1rem;
      }
  
      .coming-soon-message {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;
        font-weight: 700;
        color: var(--secondary-text);
      }
    }
  </style>
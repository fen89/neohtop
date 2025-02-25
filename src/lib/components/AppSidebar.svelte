<script>
  import Fa from "svelte-fa";
  import { faHome, faUser, faCog } from "@fortawesome/free-solid-svg-icons";

  let menuItems = [
    { name: "Home", icon: faHome, link: "/" },
    { name: "Profile", icon: faUser, link: "/profile" },
    { name: "Settings", icon: faCog, link: "/settings" },
  ];
</script>

<aside class="sidebar">
  {#each menuItems as { name, icon, link }}
    <a href={link} class="menu-item">
      <Fa {icon} class="icon" />
      <span class="label">{name}</span>
    </a>
  {/each}
</aside>

<style>
  .sidebar {
    width: 64px;
    height: 100vh;
    background: #1a1a1a;
    color: white;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 16px;
  }

  .menu-item {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 12px;
    text-decoration: none;
    color: inherit;
    transition: background 0.3s;
    border-radius: 8px;
    position: relative;
  }

  .menu-item:hover {
    background: #333;
  }

  .icon {
    font-size: 24px;
    color: #ccc;
    transition: color 0.3s;
  }

  .menu-item:hover .icon {
    color: white;
  }

  .label {
    font-size: 12px;
    margin-top: 4px;
    opacity: 0;
    transition: opacity 0.3s;
  }

  .menu-item:hover .label {
    opacity: 1;
  }
</style>

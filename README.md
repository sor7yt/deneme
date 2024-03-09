import net.minecraft.entity.LivingEntity;
import net.minecraft.entity.player.PlayerEntity;
import net.minecraft.item.Item;
import net.minecraft.item.ItemStack;
import net.minecraft.util.ActionResult;
import net.minecraft.util.Hand;
import net.minecraft.util.DamageSource;
import net.minecraft.util.math.AxisAlignedBB;
import net.minecraft.util.math.BlockPos;
import net.minecraft.world.World;

import java.util.List;

public class MagnetItem extends Item {
    public MagnetItem(Properties properties) {
        super(properties);
    }

    // Handle right-click event
    @Override
    public ActionResult<ItemStack> onItemRightClick(World world, PlayerEntity player, Hand hand) {
        double range = 10.0; // Adjust the range as needed
        MagnetUtil.attractEntities(player, player.getPosX(), player.getPosY(), player.getPosZ(), range);
        
        return super.onItemRightClick(world, player, hand);
    }

    // Handle left-click event
    @Override
    public ActionResult<ItemStack> onItemUse(World world, PlayerEntity player, Hand hand) {
        double range = 5.0; // Adjust the range as needed
        List<LivingEntity> entities = world.getEntitiesWithinAABB(LivingEntity.class, new AxisAlignedBB(player.getPosX() - range, player.getPosY() - range, player.getPosZ() - range, player.getPosX() + range, player.getPosY() + range, player.getPosZ() + range));
        
        for (LivingEntity entity : entities) {
            if (entity != player) {
                // Varlığı bayıltma işlemi
                entity.attackEntityFrom(DamageSource.GENERIC, 0.5f); // Örneğin, 0.5 zarar verme
                entity.setHealth(0); // Varlığın sağlığını sıfırlama, bayılma
            }
        }
        
        return super.onItemUse(world, player, hand);
    }
}
